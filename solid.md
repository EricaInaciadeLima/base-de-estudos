### **S – Princípio da Responsabilidade Única (SRP)**  

> Uma classe deve ter apenas uma razão para mudar.

 

**Exemplo com Spring:**

 

Imagine uma classe que lida com lógica de negócios e também com persistência de dados:

 

```java

public class UserService {

    public void registerUser(User user) {

        // lógica de registro

    }

 

    public void saveUser(User user) {

        // lógica de persistência

    }

}

```

 

**Correção com SRP:**

 

```java

@Service

public class UserService {

    public void registerUser(User user) {

        // lógica de registro

    }

}

 

@Repository

public class UserRepository {

    public void save(User user) {

        // lógica de persistência

    }

}

```

 

Agora cada classe tem uma única responsabilidade: uma cuida da lógica de negócios, outra da persistência.

 

---

 

### **O – Princípio Aberto/Fechado (OCP)**  

> Classes devem estar abertas para extensão, mas fechadas para modificação.

 

**Exemplo com Spring:**

 

Suponha que você tenha uma lógica de envio de notificações:

 

```java

public class NotificationService {

    public void send(String message) {

        System.out.println("Sending email: " + message);

    }

}

```

 

**Melhoria com OCP usando interface e Spring:**

 

```java

public interface NotificationSender {

    void send(String message);

}

 

@Component

public class EmailSender implements NotificationSender {

    public void send(String message) {

        System.out.println("Email: " + message);

    }

}

 

@Component

public class SmsSender implements NotificationSender {

    public void send(String message) {

        System.out.println("SMS: " + message);

    }

}

 

@Service

public class NotificationService {

    private final NotificationSender sender;

 

    public NotificationService(NotificationSender sender) {

        this.sender = sender;

    }

 

    public void notify(String message) {

        sender.send(message);

    }

}

```

 

Agora você pode adicionar novos tipos de envio sem modificar `NotificationService`.

 

---

 

### **L – Princípio de Substituição de Liskov (LSP)**  

> Subtipos devem ser substituíveis por seus tipos base.

 

**Exemplo com Spring:**

 

```java

public interface PaymentProcessor {

    void processPayment(double amount);

}

 

@Component

public class CreditCardProcessor implements PaymentProcessor {

    public void processPayment(double amount) {

        // lógica de pagamento com cartão

    }

}

 

@Component

public class PaypalProcessor implements PaymentProcessor {

    public void processPayment(double amount) {

        // lógica de pagamento com PayPal

    }

}

```

 

Você pode usar qualquer implementação de `PaymentProcessor` sem quebrar o sistema.

 

---

 

### **I – Princípio de Segregação de Interface (ISP)**  

> Os clientes não devem ser forçados a depender de métodos que não usam.

 

**Exemplo com Spring:**

 

```java

public interface Worker {

    void work();

    void eat();

}

```

 

Se você tiver um robô que só trabalha, ele será forçado a implementar `eat()`.

 

**Melhoria com ISP:**

 

```java

public interface Workable {

    void work();

}

 

public interface Eatable {

    void eat();

}

 

@Component

public class HumanWorker implements Workable, Eatable {

    public void work() { /*...*/ }

    public void eat() { /*...*/ }

}

 

@Component

public class RobotWorker implements Workable {

    public void work() { /*...*/ }

}

```

 

Cada classe implementa apenas o que precisa.

 

---

 

### **D – Princípio de Inversão de Dependência (DIP)**  

> Dependa de abstrações, não de implementações concretas.

 

**Exemplo com Spring:**

 

```java

public class OrderService {

    private final MySQLOrderRepository repository;

 

    public OrderService() {

        this.repository = new MySQLOrderRepository();

    }

}

```

 

**Melhoria com DIP e Spring DI:**

 

```java

public interface OrderRepository {

    void save(Order order);

}

 

@Repository

public class MySQLOrderRepository implements OrderRepository {

    public void save(Order order) { /*...*/ }

}

 

@Service

public class OrderService {

    private final OrderRepository repository;

 

    @Autowired

    public OrderService(OrderRepository repository) {

        this.repository = repository;

    }

}

```

 

Agora `OrderService` depende de uma **abstração**, facilitando testes e mudanças.

 

Os princípios **SOLID** estão **diretamente ligados** aos conceitos de **Clean Code**. Na verdade, eles são uma **extensão prática** dos valores que o Clean Code promove: **legibilidade, simplicidade, manutenibilidade e testabilidade**.

 

Vamos conectar os dois:

 

---

 

### 🔹 **Clean Code**:  

É um conjunto de boas práticas para escrever código que seja:

 

- **Fácil de entender**

- **Fácil de modificar**

- **Fácil de testar**

- **Livre de duplicações e complexidade desnecessária**

 

---

 

### 🔹 **Como SOLID se encaixa no Clean Code?**

 

| Princípio SOLID | Relação com Clean Code |

|------------------|------------------------|

| **SRP** (Responsabilidade Única) | Evita classes "Deus" que fazem tudo. Facilita leitura e manutenção. |

| **OCP** (Aberto/Fechado) | Permite evoluir o sistema sem quebrar o que já funciona. Evita retrabalho. |

| **LSP** (Substituição de Liskov) | Garante que o uso de herança não cause bugs inesperados. Mantém previsibilidade. |

| **ISP** (Segregação de Interface) | Evita interfaces inchadas. Mantém o código limpo e focado. |

| **DIP** (Inversão de Dependência) | Facilita testes e desacoplamento. Promove flexibilidade e reutilização. |

 

---

 

### 🔹 Exemplo prático com Clean Code + SOLID (Spring)

 

Imagine um sistema de pedidos:

 

```java

// Clean Code + SRP

@Service

public class OrderService {

    private final OrderRepository repository;

 

    public OrderService(OrderRepository repository) {

        this.repository = repository;

    }

 

    public void placeOrder(Order order) {

        // lógica de negócio

        repository.save(order);

    }

}

 

// Clean Code + DIP

public interface OrderRepository {

    void save(Order order);

}

 

@Repository

public class MySQLOrderRepository implements OrderRepository {

    public void save(Order order) {

        // lógica de persistência

    }

}

```

 

Esse exemplo mostra como aplicar **SRP, DIP e OCP** de forma limpa e clara, seguindo os princípios de Clean Code.

 

 

