## 🔹 O que é Flyway?

O **Flyway** é uma ferramenta de versionamento de banco de dados.

Ele permite criar **migrations**, que são arquivos SQL responsáveis por:

* Criar tabelas
* Alterar estruturas
* Adicionar colunas
* Criar índices
* Inserir dados iniciais

Ou seja, tudo que a linguagem declarativa **SQL** permite fazer.

---

## 🔹 O que são Migrations?

Cada migration é um arquivo `.sql` versionado.

Exemplo:

```
V1__create_table_cliente.sql
V2__create_table_cidade.sql
V3__create_table_endereco.sql
```

O número define a ordem de execução.

Quando a aplicação sobe, o Flyway:

1. Verifica quais versões já foram executadas
2. Executa apenas as que ainda não foram aplicadas
3. Mantém o histórico no banco

Isso garante que o banco esteja sempre sincronizado com o código.

---

## 🔹 Execução automática

Quando rodamos a aplicação com **Spring Boot**, o Flyway é executado automaticamente.

Ele:

* Conecta no banco
* Lê a pasta de migrations
* Executa os scripts em ordem sequencial

Tudo isso acontece antes da aplicação ficar disponível.

---

## 🔹 Banco utilizado: H2

No nosso projeto estamos usando o **H2**.

O H2 é um banco de dados em memória.

Isso significa:

* Não precisa instalar nada na máquina
* Ele roda junto com a aplicação
* Armazena os dados temporariamente
* Quando a aplicação para, os dados podem ser perdidos (modo memória)

---

## 🔹 Visualizando o banco

O H2 possui um console web.

Através do navegador você consegue:

* Ver as tabelas criadas
* Consultar dados
* Verificar se migrations foram aplicadas
* Executar queries manualmente

Isso ajuda muito em ambiente de desenvolvimento.

---

## 🔹 Como as duas ferramentas trabalham juntas?

✔ O Flyway é responsável por criar e versionar a estrutura do banco.
✔ O H2 é o banco que armazena os dados e permite visualizar a estrutura.

Então o fluxo é:

1. A aplicação sobe
2. O Flyway executa as migrations
3. O H2 cria as tabelas
4. Você pode acessar o console e visualizar tudo

---

> 💬 **Com o Flyway, o banco de dados deixa de ser algo manual e passa a ser código versionado, executado automaticamente sempre que a aplicação inicia.**

---

# 🔹 JPA — Ponte entre objeto e banco

`spring-boot-starter-data-jpa`

Aqui entra um conceito muito importante.

O JPA não é uma implementação.
Ele é uma **especificação**.

Ele surgiu no **Jakarta EE** (antigo Java EE).

O papel dele é:

> Fazer a ponte entre o mundo orientado a objetos e o mundo relacional.

De um lado você tem:

* Classes
* Objetos
* Herança
* Encapsulamento

Do outro lado você tem:

* Tabelas
* Colunas
* Chaves primárias
* SQL

O JPA permite mapear:

```java
@Entity
```

Para uma tabela no banco.

Ele resolve o chamado:

🔹 **ORM (Object Relational Mapping)**

No Spring Boot, quem implementa isso por baixo normalmente é o Hibernate.

Mas você programa contra a abstração (JPA), não contra a implementação.

---

# 🔹 Validation — Validação de entrada

`spring-boot-starter-validation`

Aqui entra algo muito importante.

Essa dependência implementa a especificação de validação que vem do **Jakarta EE**.

Ela é baseada na especificação:

🔹 **Bean Validation**

Ela permite usar anotações como:

```java
@NotBlank
@Email
@Size
@NotNull
```

Isso significa que:

* Você define regras declarativas
* Antes de entrar na sua regra de negócio
* O próprio framework valida o payload

### Exemplo

Se alguém mandar:

```json
{
  "email": "errado"
}
```

O sistema já responde erro automaticamente.

---

## 🎯 Qual o papel do Validation na arquitetura?

Ele atua na camada de entrada.

Ele valida:

* Se o campo é obrigatório
* Se o formato está correto
* Se o tamanho é válido
* Se é um email válido
* Se é nulo ou não

Isso evita que sua camada de serviço receba dados inválidos.

---

# 🔹 Spring Web — Camada HTTP

`spring-boot-starter-web`

Essa dependência é responsável por:

* Criar APIs REST
* Subir servidor embarcado (Tomcat)
* Trabalhar com requisições HTTP
* Mapear endpoints com `@RestController`

Ela é a porta de entrada da aplicação.

---

