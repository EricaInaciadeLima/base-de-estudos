### Primeira História!

## 📌 Cenário que você descreveu

O usuário envia um **número de dias ocorridos** → o sistema busca na tabela do banco o valor mais próximo.

Exemplo no banco:

```
dias: 125, 120, 123
entrada: 124 → retorna 125
entrada: 121 → retorna 120
```

---

## 🛠 Passo a passo no fluxo (Clean Architecture)

1. **API Layer (Controller + DTOs)**

   * O **controller** recebe a requisição com o número de dias (`GET /fundings/proximo?dias=124` ou `POST` com JSON).
   * Converte o dado recebido para um **DTO** ou valor simples e chama o caso de uso (`ObterFundingMaisProximoUseCase`).

2. **Application Layer (Use Case)**

   * O **UseCase** orquestra a operação.
   * Ele **não implementa a lógica de negócio**, apenas chama o método do domínio que sabe calcular o mais próximo.
   * Se precisar buscar do banco, chama o **Repository** (interface definida no domínio).

3. **Domain Layer (Regra de Negócio)**

   * Aqui fica a função `buscarMaisProximo(diasEntrada, listaDeDias)` que implementa a lógica:

     * Recebe a lista vinda do repositório.
     * Calcula qual é o valor mais próximo do informado.
   * Não sabe nem se a lista veio de um banco, API externa ou cache.

4. **Infrastructure Layer (Repository Implementation)**

   * Implementa `FundingRepository` usando JPA, JDBC, etc.
   * Busca do banco todos os valores de dias (ou só os mais próximos com uma query otimizada).
   * Retorna ao **UseCase** a lista para o cálculo.

5. **Retorno**

   * O resultado sobe do **Domain** → **Application** → **API**.
   * O **Controller** transforma o resultado em um **FundingResponse DTO** e devolve para o cliente.

---

## 📂 Exemplo de estrutura

```
api/
 ├─ controller/FundingController.java
 ├─ dto/FundingRequest.java
 └─ dto/FundingResponse.java

application/
 └─ usecase/ObterFundingMaisProximoUseCase.java

domain/
 ├─ model/Funding.java
 ├─ service/FundingService.java        # regra de negócio "mais próximo"
 └─ repository/FundingRepository.java  # contrato

infra/
 └─ db/
     ├─ FundingEntity.java
     ├─ FundingRepositoryImpl.java     # JPA
     └─ FundingJpaRepository.java
```

