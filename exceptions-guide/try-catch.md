
## ✅ O que é **try / catch** (definição correta)

> **`try / catch` é para LIDAR com exceções**, não apenas “tratar erros”.

“Lidar” pode significar **coisas diferentes**.

---

## 🧠 O que “tratar” realmente significa

Quando você usa `try / catch`, você está dizendo:

> “Se algo der errado aqui, eu assumo a responsabilidade.”

E assumir responsabilidade pode ser:

### ✔️ 1️⃣ Resolver o problema

```java
try {
    lerArquivo();
} catch (IOException e) {
    criarArquivoPadrao();
}
```

---

### ✔️ 2️⃣ Traduzir a exceção

```java
try {
    repository.salvar();
} catch (SQLException e) {
    throw new NegocioException("Erro ao salvar", e);
}
```

---

### ✔️ 3️⃣ Adicionar contexto (log) e relançar

```java
try {
    executar();
} catch (Exception e) {
    log.error("Falha ao executar pedido {}", id, e);
    throw e;
}
```

---

## ❌ O que NÃO é tratamento (erro comum)

```java
try {
    executar();
} catch (Exception e) {
    log.error(e.getMessage());
}
```

🚨 Isso **não é tratamento**
🚨 Isso é **engolir erro**

---

## 🔥 Frase-chave para memorizar

> **Capturar exceção sem resolver ou relançar NÃO é tratamento.**

---

## 📍 Relação com erro 500

* Exceção **não tratada** → sobe → framework → **500**
* Exceção **tratada de verdade** → pode virar:

  * resposta controlada
  * outro fluxo
  * outro tipo de erro (400, 404…)

📌 `try/catch` **não cria erro 500**
📌 Ele só decide **se o erro continua ou para**

---

## 🧪 Em testes unitários

* Se você quer testar erro:

  * **a exceção precisa subir**
* Se você capturar e não relançar:

  * o teste **não vê erro**

---

## 🧠 Resumo final

* ✔️ `try/catch` serve para **lidar com exceções**
* ✔️ Lidar pode ser:

  * resolver
  * traduzir
  * relançar
* ❌ Engolir exceção não é tratar
* ✔️ Erro 500 nasce quando a exceção sobe

---

## 💬 Frase definitiva

> **`try/catch` é um ponto de decisão:
> ou você resolve o erro, ou deixa ele continuar.**

Se quiser, posso:

* revisar um `try/catch` seu
* mostrar exemplos certos vs errados
* ligar isso diretamente com seus testes unitários
