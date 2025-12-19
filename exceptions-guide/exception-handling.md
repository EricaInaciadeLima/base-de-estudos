

# 📘 Exception Handling (Handling de Exceções)

## 🎯 Objetivo

Este documento explica o conceito de **exception handling**, o papel do `try/catch`, do `throw` e do **global handling**, e como isso se relaciona com **erros HTTP (ex: 500)** e testes unitários.

---

## 🧠 O que são exceções

Exceções representam **situações inesperadas ou inválidas** que ocorrem durante a execução da aplicação, como por exemplo:

* dados inválidos
* recurso não encontrado
* falha de integração
* erro interno inesperado

Quando uma exceção ocorre, ela é **lançada (`throw`)** automaticamente ou manualmente.

📌 **Importante:**
Exceções **não ficam ocultas por padrão**.
Elas **só ficam ocultas se alguém capturar (`catch`) e não relançar**.

---

## 🔁 Fluxo natural de uma exceção

1️⃣ Um erro acontece
2️⃣ Uma exceção é lançada (`throw`)
3️⃣ A exceção sobe pela pilha
4️⃣ Alguém decide lidar com ela (**handling**)
5️⃣ Se ninguém lidar, o framework web converte em **HTTP 500**

---

## 🧩 O papel do `try / catch`

O `try/catch` **não existe apenas para “evitar erro”**.

Ele existe para **ASSUMIR RESPONSABILIDADE** quando algo dá errado.

### Quando você usa `try/catch`, você está dizendo:

> “Se der erro aqui, eu sei o que fazer.”

---

## ✅ O que é um tratamento correto (handling)

Tratar (handle) uma exceção significa **fazer uma escolha clara**.

### 1️⃣ Resolver o problema

```text
Erro aconteceu → resolvo → sigo o fluxo
```

### 2️⃣ Traduzir a exceção

```text
Erro técnico → erro de negócio mais claro
```

### 3️⃣ Relançar com contexto

```text
Erro aconteceu → adiciono contexto → lanço novamente
```

📌 **Relançar (`throw`) NÃO é erro**
📌 É uma decisão consciente

---

## ❌ O que NÃO é handling (erro comum)

```text
capturar erro → logar → não relançar
```

Isso:

* esconde o problema
* impede erro HTTP
* quebra testes
* dificulta debugging

### ## 📍 Handling Local: onde ele acontece e como identificar

O **handling local** acontece quando, dentro de uma camada específica (geralmente o *service*), o código **assume a responsabilidade** por uma exceção e **toma uma decisão explícita** sobre ela.

Essa decisão normalmente ocorre **no bloco `catch`**.

### Exemplo conceitual

```java
try {
    executarRegra();
} catch (RegraInvalidaException e) {
    throw new RecursoNaoEncontradoException("Recurso não encontrado", e);
}
```

Neste exemplo, o handling local ocorre porque:

* a exceção original foi **capturada**
* foi feita uma **decisão de significado**
* a exceção foi **traduzida** para outra mais adequada ao contexto
* a exceção foi **relançada (`throw`)**

📌 **O handling NÃO está na string da mensagem**
📌 **O handling está na decisão tomada no `catch`**

---

### 🧠 O que o Service está fazendo nesse handling

Ao executar esse handling local, o service está dizendo:

> “Para mim, esse erro técnico significa que o recurso não existe.”

O service:

* **não define HTTP status**
* **não conhece REST**
* **não conhece controller**
* apenas **expressa o significado do erro**

---


🚨 **Isso NÃO é tratar exceção**

---

## 🔥 Ajuste importante no seu entendimento

### ❌ Ideia incorreta

> “Se eu não tratar, o erro fica oculto”

### ✅ Correto

> **Se eu capturar e não relançar, o erro fica oculto**

Sem `try/catch`, a exceção **sobe sozinha**.

---

## 🌐 O que é Global Exception Handling

Sim 👍 aqui seu entendimento está **bem alinhado**, só vamos deixar mais técnico.

### Global handling é:

> Um **ponto central da aplicação** responsável por:

* capturar exceções
* mapear exceções → HTTP Status
* padronizar mensagens de erro

Exemplo conceitual:

| Tipo de exceção        | HTTP Status |
| ---------------------- | ----------- |
| RegraNegocioException  | 400         |
| RecursoNaoEncontrado   | 404         |
| Qualquer outra exceção | 500         |

📌 O controller **não decide** o status
📌 O service **não retorna HTTP**
📌 O global handler faz essa tradução

---

## 🔗 Relação entre Service, Controller e Handling

### Service

* lança exceções
* não conhece HTTP
* não decide status

### Controller

* chama o service
* não trata exceção na maioria dos casos

### Global Handler

* captura exceções
* define status HTTP
* define mensagem padronizada

---

## 🧪 Impacto em testes unitários

Para testar erro (ex: 500):

* a exceção **precisa subir**
* o erro **não pode ser engolido**
* o mock pode simular a exceção
* o teste valida o comportamento

📌 Você **não altera a implementação real só para o teste**

---

## 🧠 Resumo mental (regra de ouro)

* `throw` → lança exceção
* `catch` → captura exceção
* `handling` → decide o que fazer
* global handler → traduz exceção em HTTP
* erro 500 → exceção não tratada pela aplicação

---

## 💬 Frase final para memorizar

> **Handling não é evitar erro.
> Handling é decidir, de forma explícita, como o erro será visto.**

