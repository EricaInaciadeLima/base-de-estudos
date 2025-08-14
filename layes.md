

### **1. ValueObject – Funding**

* Esse é um **objeto de valor** (no DDD – Domain-Driven Design) que representa um conceito importante no domínio, neste caso "Funding" (financiamento).
* Ele pertence à camada **Domínio** e é imutável (só pode ser criado com valores válidos).

---

### **2. Dominio (Domain)**

* É a camada **central** das regras de negócio.
* Aqui ficam **entidades**, **value objects** e **regras de negócio puras**, sem dependência de infraestrutura ou frameworks.
* Recebe informações e comandos da **Aplicação** e pode usar dados vindos da **Infraestrutura**.

---

### **3. Aplicação (Application)**

* Contém a **lógica de orquestração** da aplicação (sem regras de negócio complexas).
* Executa **casos de uso** e coordena chamadas para o domínio e infraestrutura.

---

### **4. UseCases (CQRS)**

* São implementações específicas de **casos de uso** (ex.: `ObterFundingUseCase`).
* CQRS (Command Query Responsibility Segregation) indica separação entre comandos (alterações) e queries (consultas).
* Fica entre a **API** e a **Aplicação** para processar a solicitação.

---

### **5. API (Presentation)**

* É a camada de **apresentação**, onde ficam endpoints HTTP, GraphQL etc.
* Recebe as requisições do mundo externo e as encaminha para os casos de uso.

---

### **6. Controllers – FundingController**

* Controladores específicos (ex.: `FundingController`) que recebem requisições da API, validam, e chamam o caso de uso correspondente.

---

### **7. Infraestrutura (Infrastructure)**

* Implementa detalhes técnicos como persistência, acesso a banco de dados, integração com APIs externas, etc.
* Interage com o **Repository** para buscar ou gravar dados.

---

### **8. Repository**

* Interface (definida no domínio ou aplicação) e implementação (na infraestrutura) para acessar dados.
* Encapsula consultas ao banco ou APIs externas.

---

📌 **Fluxo resumido do exemplo (`ObterFundingUseCase`):**

1. O usuário chama um endpoint → **Controller** (FundingController).
2. O Controller aciona o **UseCase** (`ObterFundingUseCase`).
3. O UseCase chama a **Aplicação**.
4. A Aplicação acessa o **Domínio** (regras de negócio).
5. Se precisar de dados, a Aplicação chama a **Infraestrutura** via **Repository**.
6. O resultado sobe de volta pela cadeia até a API responder.

