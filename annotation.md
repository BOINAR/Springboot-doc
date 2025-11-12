📘 Spring Boot — Guide Propre des Annotations Essentielles

Ce fichier regroupe toutes les annotations Spring Boot essentielles, organisées proprement, avec explications claires et exemples.

⸻

🔥 1. Annotations fondamentales de Spring

✅ @SpringBootApplication

Regroupe 3 annotations :
	•	@Configuration
	•	@EnableAutoConfiguration
	•	@ComponentScan

C’est l’annotation principale qui démarre votre application.

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}


⸻

🧱 2. Annotations de composants (Beans Spring)

Spring gère automatiquement ces classes dans son conteneur de beans.

🔸 @Component

Composant générique. Souvent remplacé par @Service ou @Repository.

@Component
public class EmailValidator {}

🔸 @Service

Indique une classe de logique métier.

@Service
public class UserService {}

🔸 @Repository

Couche d’accès aux données.
Ajoute une conversion automatique des exceptions SQL.

@Repository
public interface UserRepository {}

🔸 @Controller

Contrôleur MVC (retourne des pages HTML).

@Controller
public class HomeController {}

🔸 @RestController

Combinaison de :
	•	@Controller
	•	@ResponseBody

Retourne automatiquement du JSON.

@RestController
@RequestMapping("/api/users")
public class UserController {}


⸻

🔄 3. Injection de dépendances (DI)

⭐ @Autowired

Injecte automatiquement un bean Spring.

✔ Injection par constructeur (recommandée)

@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }
}

✖ Injection par champ (déconseillée)

@Autowired
private UserRepository repo;

📌 @Qualifier

Utilisé lorsqu’il y a plusieurs beans du même type.

@Autowired
@Qualifier("paymentStripe")
private PaymentService service;

📌 @Value

Injection de valeurs depuis application.properties.

@Value("${app.version}")
private String version;


⸻

🛠 4. Annotations JPA / Hibernate (ORM)

🏷 @Entity

Indique que la classe représente une table.

@Entity
public class User {}

🏷 @Table

Permet de préciser le nom de la table.

@Table(name = "users")

🔑 @Id

Clé primaire.

🔧 @GeneratedValue

Stratégie d’auto-incrémentation.

@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;

🧩 @Column

Configuration d’une colonne.

@Column(nullable=false, unique=true)
private String email;

🔗 Relations:

@OneToOne

@OneToMany

@ManyToOne

@ManyToMany

Exemple :

@OneToMany(mappedBy="user")
private List<Order> orders;


⸻

🌐 5. Annotations pour les API REST

🔸 @RequestMapping

Définit la racine d’un contrôleur.

@RequestMapping("/api/users")

🔹 @GetMapping, @PostMapping, @PutMapping, @DeleteMapping

Routes HTTP.

@GetMapping("/{id}")
public User getById(@PathVariable Long id) {}

📌 @PathVariable

Extrait une variable d’URL.

@GetMapping("/user/{id}")
public User get(@PathVariable Long id) {}

📌 @RequestParam

Paramètre de requête.

@GetMapping
public List<User> search(@RequestParam String name) {}

📌 @RequestBody

Récupère un JSON envoyé par le client.

@PostMapping
public User create(@RequestBody User user) {}


⸻

🛡 6. Validation (Bean Validation)

Activer avec :

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>

🟦 Annotations de validation

Annotation	Description
@NotNull	Ne doit pas être null
@NotBlank	Texte non vide
@Email	Format email
@Min / @Max	Valeurs min / max
@Size	Taille min/max

Exemple :

public class UserDTO {
    @NotBlank
    private String name;

    @Email
    private String email;
}

Et dans un contrôleur :

@PostMapping
public User create(@Valid @RequestBody UserDTO dto) {}


⸻

🎯 7. Annotations de configuration

🔧 @Configuration

Déclare une classe de configuration.

🔧 @Bean

Retourne manuellement un bean à intégrer dans le conteneur Spring.

@Bean
public PasswordEncoder encoder() {
    return new BCryptPasswordEncoder();
}

🔧 @EnableScheduling

Active les tâches planifiées.

🔧 @Scheduled

Exécute une tâche automatiquement.

@Scheduled(fixedRate = 60000)
public void runEveryMinute() {}


⸻

🏁 Conclusion

Ce fichier regroupe toutes les annotations essentielles pour travailler proprement avec Spring Boot :
	•	structure
	•	injection
	•	ORM
	•	REST API
	•	validation
	•	configuration avancée

Si tu veux, je peux maintenant générer :
👉 un fichier complet sur l’injection de dépendances
👉 un fichier complet sur JPA & équivalent LINQ
👉 un fichier sur l’architecture complète d’un projet Spring Boot
