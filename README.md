# Springboot-doc

📘 Documentation Spring Boot

Bienvenue dans ta documentation Spring Boot. Ce fichier regroupe tout ce que tu as appris : injections, annotations, JPA/Hibernate, équivalent LINQ, architecture, etc.

⸻

🚀 Introduction à Spring Boot

Spring Boot est un framework Java qui simplifie la création d’applications.

Il apporte :
	•	un conteneur d’inversion de contrôle (IoC)
	•	un système d’auto-configuration
	•	un serveur intégré
	•	une gestion automatique des dépendances

⸻

🗂️ Structure d’un projet Spring Boot

src/
 ├── main/
 │    ├── java/com/.../project/
 │    │    ├── controller/
 │    │    ├── service/
 │    │    ├── repository/
 │    │    ├── model/
 │    │    └── config/
 │    └── resources/
 │         ├── application.properties
 │         └── static/
 └── test/


⸻

🧵 Annotations & Beans

Annotation	Rôle
@Component	Composant générique géré par Spring
@Service	Logique métier
@Repository	Couche accès aux données
@Controller	MVC
@RestController	API REST
@Configuration	Classe de configuration
@Bean	Déclare un bean manuellement


⸻

🔄 Injection de dépendances (DI)

➤ @Autowired

Permet à Spring d’injecter automatiquement un objet dans une classe.

Injection par constructeur (recommandée)

@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }
}

Injection par champ (déconseillée)

@Autowired
private UserRepository repo;


⸻

🧩 JPA / Hibernate (ORM)

🔧 Dépendances Maven

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>


⸻

⚙️ Configuration de la base de données

application.properties :

spring.datasource.url=jdbc:postgresql://localhost:5432/app
spring.datasource.username=postgres
spring.datasource.password=mdp
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true


⸻

🧱 Entité JPA

@Entity
@Table(name="users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String email;
    private String name;
}


⸻

🏦 Repository (équivalent LINQ)

✔ Méthodes générées automatiquement (Spring Data JPA)

public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
    List<User> findByNameContainingIgnoreCase(String name);
}

✔ JPQL (équivalent LINQ avancé)

@Query("SELECT u FROM User u WHERE u.age > :age")
List<User> findOlderThan(@Param("age") int age);

✔ SQL natif

@Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
User findByEmailNative(String email);


⸻

🧠 Services

@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }

    public List<User> getAll() {
        return repo.findAll();
    }
}


⸻

🌐 Contrôleurs REST

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping
    public List<User> getAll() {
        return service.getAll();
    }
}


⸻

🧪 Tests (à venir)
	•	JUnit
	•	MockMvc
	•	Mockito

⸻

📦 Bonnes pratiques
	•	Utiliser l’injection par constructeur
	•	Organiser le projet par couches (Controller/Service/Repository)
	•	Ne jamais mettre de logique dans les contrôleurs
	•	Utiliser des DTO (MapStruct recommandé)
	•	Valider les entrées utilisateur (@Valid, @NotNull…)

⸻

✔️ À ajouter plus tard
	•	Pagination / Pageable
	•	Spring Security
	•	Gestion globale des erreurs
	•	DTO + MapStruct

⸻

🎉 Fin de la documentation de base

Tu peux maintenant étoffer ce fichier ou me demander de générer d’autres fichiers (ex: 01_injection.md, 02_jpa.md, 03_controllers.md).
