# Spring Boot — Guide des Annotations Essentielles (Version Détaillée)

Ce document contient **des explications claires, pédagogiques et approfondies** sur chaque annotation Spring Boot, avec exemples, contexte, bonnes pratiques, et usage réel.

---

# 🔥 1. Annotation principale : démarrage de l'application

## `@SpringBootApplication`
C’est **l’annotation la plus importante de Spring Boot**. Elle est placée sur la classe principale et indique à Spring :

✔ "Ceci est une application Spring Boot"
✔ "Configure tout automatiquement"
✔ "Scanne les composants dans le projet"

Elle regroupe 3 annotations :
- **`@Configuration`** → indique que cette classe contient des beans
- **`@EnableAutoConfiguration`** → Spring configure automatiquement ce dont tu as besoin
- **`@ComponentScan`** → recherche automatiquement les classes annotées (`@Service`, `@Repository`, etc.)

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

🎯 **À retenir :** Tu n'as rien à configurer manuellement. Spring Boot s'occupe de tout.

---

# 🧱 2. Annotations de composants (Beans Spring)
Spring gère des "composants" appelés **beans**. Un bean = un objet instancié et géré automatiquement.

Ces annotations permettent à Spring de détecter ces classes.

## `@Component`
Annotation générique pour créer un bean Spring.  
Peu utilisée directement car il existe des variantes plus spécifiques (`@Service`, `@Repository`, etc.).

```java
@Component
public class EmailValidator {}
```

## `@Service`
Indique que la classe contient **la logique métier**.  
Exemple : créer un utilisateur, envoyer un mail, appliquer une réduction, etc.

```java
@Service
public class UserService {}
```

🎯 **Meilleure pratique :** La logique métier VA TOUJOURS dans les services, jamais dans les contrôleurs.

## `@Repository`
Représente **la couche d'accès aux données** (ORM / SQL).  
Spring ajoute automatiquement :
- gestion des exceptions SQL
- intégration avec JPA/Hibernate

```java
@Repository
public interface UserRepository {}
```

## `@Controller`
Utilisé lorsque tu génères des vues HTML (Thymeleaf). Rare en API REST.

## `@RestController`
Version spéciale pour les API REST.  
Retourne **automatiquement du JSON**.

```java
@RestController
@RequestMapping("/api/users")
public class UserController {}
```

🎯 **À retenir :** C’est l’annotation que tu utiliseras 90% du temps pour une API.

---

# 🔄 3. Injection de dépendances (DI)
La DI permet à Spring de **créer et injecter automatiquement les objets dont une classe a besoin**.

Spring s’en occupe : **tu n'as jamais besoin de faire `new` manuellement.**

## `@Autowired`
Demande à Spring d'injecter un bean.

### ✔ Injection par constructeur (LA meilleure façon)
```java
@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) { // injection automatique
        this.repo = repo;
    }
}
```

Pourquoi c’est mieux :
- immuable (bon design)
- facile à tester
- recommandé par Spring
- fonctionne sans `@Autowired` (Spring l’infère automatiquement)

### ✖ Injection par champ (à éviter)
```java
@Autowired
private UserRepository repo;
```

❌ Difficile à tester, moins clair, déconseillé par Spring.

## `@Qualifier`
Utilisé quand plusieurs beans existent du même type.

```java
@Autowired
@Qualifier("stripePayment")
private PaymentService service;
```

## `@Value`
Injection de valeurs depuis les fichiers :  
`application.properties` ou `application.yml`

```java
@Value("${app.version}")
private String version;
```

---

# 🛠 4. Annotations JPA / Hibernate (ORM)
Elles permettent de mapper des classes Java à une base SQL.

---

## `@Entity`
Indique que cette classe est une **table SQL**.

```java
@Entity
public class User {}
```

## `@Table`
Définit le nom de la table.

```java
@Table(name = "users")
```

## `@Id`
Clé primaire.

## `@GeneratedValue`
Génération automatique de l’ID.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

## `@Column`
Configurer une colonne : unique, nullable, longueur max…

```java
@Column(nullable = false, unique = true)
private String email;
```

## Relations JPA
Spring gère les relations entre les tables :

### `@OneToMany`
Un utilisateur → plusieurs commandes.

```java
@OneToMany(mappedBy = "user")
private List<Order> orders;
```

### `@ManyToOne`
Plusieurs commandes → un utilisateur.

### `@OneToOne`
Relation 1-1.

### `@ManyToMany`
Relation plusieurs ↔ plusieurs.

---

# 🌐 5. Annotations pour les API REST

## `@RequestMapping`
Route principale du contrôleur.

```java
@RequestMapping("/api/users")
```

## `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`

```java
@GetMapping("/{id}")
public User getById(@PathVariable Long id) {}
```

## `@PathVariable`
Extrait une variable de l’URL.

```java
@GetMapping("/user/{id}")
public User get(@PathVariable Long id) {}
```

## `@RequestParam`
Lit une variable dans l’URL : `?name=paul`

```java
@GetMapping
public List<User> search(@RequestParam String name) {}
```

## `@RequestBody`
Récupère un JSON envoyé par le client (POST, PUT).

```java
@PostMapping
public User create(@RequestBody User user) {}
```

---

# 🛡 6. Validation (Bean Validation)
Permet de valider automatiquement les données entrantes.

## Exemple : DTO validé
```java
public class UserDTO {
    @NotBlank
    private String name;

    @Email
    private String email;

    @Min(18)
    private int age;
}
```

## Utilisation dans un contrôleur
```java
@PostMapping
public User create(@Valid @RequestBody UserDTO dto) {}
```

---

# 🎛 7. Annotations de configuration

## `@Configuration`
Classe qui contient des beans personnalisés.

## `@Bean`
Permet de déclarer un bean manuellement.

```java
@Bean
public PasswordEncoder encoder() {
    return new BCryptPasswordEncoder();
}
```

## `@EnableScheduling` / `@Scheduled`
Pour exécuter des tâches automatiques.

```java
@EnableScheduling
public class Config {}

@Scheduled(fixedRate = 60000)
public void run() {}
```

---

## relation
exemple de relation dans le model

```java
@ManyToOne(optional = false)
@JoinColumn(name = "user_id")
private User user;
```

---

## Enumération
exemple d'enumération

```java
@Enumerated(EnumType.STRING)
private AuthProvider provider;
```

🏁 Conclusion
Tu as maintenant un fichier :

✔ Très bien structuré  
✔ Clair et pédagogique  
✔ Avec des explications détaillées  
✔ Équivalent à une vraie documentation pro

Si tu veux, je peux générer :
👉 un fichier Markdown complet sur **JPA et l’équivalent de LINQ en Spring Boot**  
👉 ou sur **l’architecture complète d’un projet Spring Boot**  
👉 ou sur **les services / contrôleurs / requêtes ORM**

Dis-moi ce que tu veux en suivant !
