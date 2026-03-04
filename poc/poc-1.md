
---

# 🧱 2️⃣ Estrutura Geral das Camadas

Uma aplicação bem estruturada normalmente tem:

1. Controller (Camada de Entrada)
2. Service (Camada de Aplicação)
3. Repository (Persistência)
4. Entidade (Domínio)
5. DTO (Transporte de Dados)

---

# 🟦 3️⃣ Controller – Camada mais próxima do usuário

O Controller:

* Recebe requisições HTTP
* Valida dados de entrada
* Chama o Service
* Retorna resposta para o usuário

Ele não deve conter regra de negócio.

Ele apenas controla o fluxo.

📌 Ele responde:

> Quem está chamando e qual operação deseja executar?

---

# 🟩 4️⃣ Service – Camada de Aplicação

O Service:

* Orquestra o caso de uso
* Chama repositórios
* Chama métodos das entidades
* Controla transações

Ele executa o fluxo da aplicação.

📌 Ele responde:

> Como esse caso de uso deve acontecer?

Exemplo:
Criar cliente.
Adicionar endereço.
Bloquear cliente.

Mas ele não deve conter regras internas da entidade.

---

# 🟨 5️⃣ Entidade – Camada de Domínio

A Entidade representa o modelo do negócio.

Ela:

* Possui atributos
* Possui comportamentos
* Mantém consistência interna

Exemplo de comportamento de domínio:

* adicionarTelefone
* removerTelefone
* bloquearCliente
* alterarSenha

Ela não sabe que existe banco de dados.

📌 Ela responde:

> O que eu posso fazer como conceito de negócio?

---

# 🟫 6️⃣ Repository – Persistência

O Repository:

* Salva
* Busca
* Atualiza
* Deleta no banco

Ele é responsável apenas pela comunicação com o banco.

Ele não contém regra de negócio.

📌 Ele responde:

> Como armazenar e recuperar dados?

---

# 🟪 7️⃣ DTO – Transporte de Dados

DTO significa Data Transfer Object.

Ele serve para:

* Transportar dados entre camadas
* Evitar expor entidade diretamente
* Controlar o formato da entrada e saída

Ele não contém regra de negócio.
Ele é apenas estrutura de dados.

---

# 🔄 8️⃣ Fluxo Completo de Uma Requisição

1. Cliente faz requisição HTTP
2. Controller recebe
3. Controller chama Service
4. Service executa regra
5. Service chama Repository
6. Repository salva/busca
7. Resultado volta pelo mesmo caminho

---

# 🧠 9️⃣ Separação de Responsabilidades

Cada camada tem uma responsabilidade única.

Controller → comunicação
Service → orquestração
Entidade → regra de negócio
Repository → persistência
DTO → transporte

Isso deixa o sistema:

* Organizado
* Testável
* Escalável
* Fácil de manter


