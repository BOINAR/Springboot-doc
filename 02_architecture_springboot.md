# 02 — Architecture Spring Boot (Version Complète et Explicative)

Ce document explique en profondeur **l’architecture standard d’un projet Spring Boot**, comment organiser ton code, et comment fonctionne une requête HTTP de A à Z.

---

# 🎯 Objectif
Comprendre :
- comment structurer un projet Spring Boot
- le rôle exact de chaque couche (controller/service/repository/entity)
- le chemin parcouru par une requête
- les bonnes pratiques utilisées en entreprise

---

# 🔥 1. L’architecture en couches (le cœur de Spring Boot)

Spring Boot utilise une architecture en **couches séparées**, aussi appelée "layered architecture".

Voici la structure :

```
Controller → Service → Repository → Database
```

Chaque couche a un rôle très spécifique.

---

# 🧱 2. Les couches en détail

## 🟦 Controller : la couche Web (entrée/sortie)
- reçoit les requêtes HTTP
- prépare la réponse (JSON)
- appelle le service approprié
- NE contient PAS de logique métier

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping
    public List<User> getUsers() {
        return service.findAll();
    }
}
```

---

## 🟨 Service : la logique métier
C’est **le cerveau de l’application**.

Le service :
- applique les règles métier
- valide les données
- appelle les repositories
- orchestre les opérations

```java
@Service
public class UserService {

    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }

    public List<User> findAll() {
        return repo.findAll();
    }
}
```

---

## 🟩 Repository : l’accès aux données (ORM)
C’est ici que tu interagis avec la base SQL.

Le repository :
- exécute les requêtes SQL via l’ORM
- renvoie les entités (objets représentant les tables)

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {}
```

Grâce à `JpaRepository`, tu obtiens :
- `findAll()`
- `findById()`
- `save()`
- `deleteById()`
- et bien plus… automatiquement.

---

## 🟥 Entities : la représentation des tables
Les entités sont des classes Java annotées avec `@Entity`.

Elles :
- correspondent à une table SQL
- contiennent les colonnes sous forme d’attributs

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

---

# 🔄 3. Le flux complet d’une requête HTTP

Quand un client appelle `/api/users` :

```
[Client] → Controller → Service → Repository → Database
```

Pour la réponse :

```
Database → Repository → Service → Controller → JSON
```

Ce schéma est **fondamental** — il faut le connaître par cœur.

---

# 🏗 4. Arborescence complète d’un projet Spring Boot

Voici l’architecture recommandée pour un projet propre :

```
src/
 └── main/
      └── java/com.project.example/
             ├── controller/
             │    └── UserController.java
             ├── service/
             │    └── UserService.java
             ├── repository/
             │    └── UserRepository.java
             ├── model/
             │    └── User.java
             └── dto/
                  └── UserDTO.java
```

Chaque dossier a un rôle clair et séparé.

---

# 🧠 5. Pourquoi cette architecture est obligatoire ?

✔ Évite d’avoir du code spaghetti  
✔ Permet de tester facilement  
✔ Rend ton code lisible par toute l’équipe  
✔ Facilite la maintenance  
✔ Respecte les standards Spring

En entreprise, **une API qui ne respecte pas cette architecture ne passe pas les revues de code**.

---

# 📚 6. Exemple complet : Controller → Service → Repository → Entity

### ENTITY
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

### REPOSITORY
```java
public interface UserRepository extends JpaRepository<User, Long> {}
```

### SERVICE
```java
@Service
public class UserService {

    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }

    public List<User> findAll() {
        return repo.findAll();
    }
}
```

### CONTROLLER
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping
    public List<User> getUsers() {
        return service.findAll();
    }
}
```

---

# 🧩 7. Les règles d’or de l’architecture Spring Boot

### ❌ À NE JAMAIS FAIRE
- mettre la logique métier dans les controllers
- faire du SQL dans les services
- exposer directement les entités dans les réponses JSON
- mélanger les couches

### ✔ À TOUJOURS FAIRE
- logique métier → **Service**
- accès DB → **Repository**
- réponses JSON → **Controller**
- mapping → **DTO** (plus tard)

---

# 🏁 Conclusion
Tu as maintenant une vision claire et complète de :
- comment structurer un projet Spring Boot
- le rôle précis de chaque couche
- le chemin complet d’une requête
- les bonnes pratiques utilisées par les développeurs professionnels

Prochaine étape :  
📘 **03 — Spring Data JPA : Requêtes, ORM, Équivalent LINQ, JPQL**

Dis-moi quand tu veux que je génère le fichier suivant !
