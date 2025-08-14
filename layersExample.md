Claro, Erica! Aqui está a explicação **refeita com exemplos usando Spring Boot (Java)** para cada camada da arquitetura DDD + CQRS:

 

---

 

### **1. ValueObject – Funding**

 

* Um **Value Object** representa um conceito com valor, mas **sem identidade própria**.

* Exemplo:

```java

public class Funding {

    private final BigDecimal valor;

    private final String moeda;

 

    public Funding(BigDecimal valor, String moeda) {

        if (valor.compareTo(BigDecimal.ZERO) <= 0) {

            throw new IllegalArgumentException("Valor deve ser positivo.");

        }

        this.valor = valor;

        this.moeda = moeda;

    }

 

    public BigDecimal getValor() { return valor; }

    public String getMoeda() { return moeda; }

}

```

 

---

 

### **2. Domínio (Domain)**

 

* Contém **entidades** e **regras de negócio**.

* Exemplo de entidade:

```java

public class Projeto {

    private UUID id;

    private String nome;

    private Funding financiamento;

 

    public Projeto(String nome, Funding financiamento) {

        this.id = UUID.randomUUID();

        this.nome = nome;

        this.financiamento = financiamento;

    }

 

    public UUID getId() { return id; }

    public Funding getFinanciamento() { return financiamento; }

}

```

 

---

 

### **3. Aplicação (Application)**

 

* Coordena o fluxo entre as camadas.

* Exemplo:

```java

@Service

public class ProjetoService {

    private final ProjetoRepository repository;

 

    public ProjetoService(ProjetoRepository repository) {

        this.repository = repository;

    }

 

    public Projeto obterProjeto(UUID id) {

        return repository.findById(id).orElseThrow(() -> new RuntimeException("Projeto não encontrado"));

    }

}

```

 

---

 

### **4. UseCases (CQRS)**

 

* Separa **comandos** (alterações) de **consultas**.

* Exemplo de consulta:

```java

@Service

public class ObterFundingUseCase {

    private final ProjetoService projetoService;

 

    public ObterFundingUseCase(ProjetoService projetoService) {

        this.projetoService = projetoService;

    }

 

    public Funding execute(UUID projetoId) {

        Projeto projeto = projetoService.obterProjeto(projetoId);

        return projeto.getFinanciamento();

    }

}

```

 

---

 

### **5. API (Presentation)**

 

* Recebe requisições externas.

* Exemplo:

```java

@RestController

@RequestMapping("/projetos")

public class FundingController {

    private final ObterFundingUseCase useCase;

 

    public FundingController(ObterFundingUseCase useCase) {

        this.useCase = useCase;

    }

 

    @GetMapping("/{id}/funding")

    public ResponseEntity<Funding> getFunding(@PathVariable UUID id) {

        Funding funding = useCase.execute(id);

        return ResponseEntity.ok(funding);

    }

}

```

 

---

 

### **6. Controllers – FundingController**

 

* Controla a entrada da API e chama o caso de uso.

* Já mostrado acima no exemplo da API.

 

---

 

### **7. Infraestrutura (Infrastructure)**

 

* Implementa acesso a banco, APIs, etc.

* Exemplo com Spring Data JPA:

```java

@Repository

public interface ProjetoRepository extends JpaRepository<ProjetoEntity, UUID> {

}

```

 

* E a entidade JPA:

```java

@Entity

public class ProjetoEntity {

    @Id

    private UUID id;

    private String nome;

    private BigDecimal valor;

    private String moeda;

 

    // Getters e setters...

}

```

 

---

 

### **8. Repository**

 

* Interface e implementação para acessar dados.

* Interface no domínio:

```java

public interface ProjetoRepository {

    Optional<Projeto> findById(UUID id);

}

```

 

* Implementação na infraestrutura:

```java

@Repository

public class ProjetoRepositoryImpl implements ProjetoRepository {

    private final ProjetoJpaRepository jpaRepository;

 

    public ProjetoRepositoryImpl(ProjetoJpaRepository jpaRepository) {

        this.jpaRepository = jpaRepository;

    }

 

    @Override

    public Optional<Projeto> findById(UUID id) {

        return jpaRepository.findById(id).map(entity ->

            new Projeto(entity.getNome(), new Funding(entity.getValor(), entity.getMoeda()))

        );

    }

}

```