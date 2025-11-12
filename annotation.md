# Spring Boot — Guide des Annotations Essentielles

## 1. Annotation principale

### `@SpringBootApplication`
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

---

## 2. Annotations de composants (Beans Spring)

### `@Component`
```java
@Component
public class EmailValidator {}
```

### `@Service`
```java
@Service
public class UserService {}
```

### `@Repository`
```java
@Repository
public interface UserRepository {}
```

### `@Controller`
```java
@Controller
public class PageController {}
```

### `@RestController`
```java
@RestController
@RequestMapping("/api/users")
public class UserController {}
```

---

## 3. Injection de dépendances (DI)

### `@Autowired`
#### Injection par constructeur (recommandée)
```java
@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }
}
```

#### Injection par champ (déconseillée)
```java
@Autowired
private UserRepository repo;
```

### `@Qualifier`
```java
@Autowired
@Qualifier("stripePayment")
private PaymentService service;
```

### `@Value`
```java
@Value("${app.version}")
private String version;
```

---

## 4. Annotations JPA / Hibernate (ORM)

### `@Entity`
```java
@Entity
public class User {}
```

### `@Table`
```java
@Table(name = "users")
```

### `@Id` + `@GeneratedValue`
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

### `@Column`
```java
@Column(nullable = false, unique = true)
private String email;
```

### Relations JPA
```java
@OneToMany(mappedBy = "user")
private List<Order> orders;
```

---

## 5. API REST — Annotations Web

### `@RequestMapping`
```java
@RequestMapping("/api/users")
```

### `@GetMapping`, `@PostMapping`, etc.
```java
@GetMapping("/{id}")
public User getById(@PathVariable Long id) {}
```

### `@PathVariable`
```java
@GetMapping("/user/{id}")
public User get(@PathVariable Long id) {}
```

### `@RequestParam`
```java
@GetMapping
public List<User> search(@RequestParam String name) {}
```

### `@RequestBody`
```java
@PostMapping
public User create(@RequestBody User user) {}
```

---

## 6. Validation (Hibernate Validator)

### Dépendance Maven
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
