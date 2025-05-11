# **Java Spring Boot - Complete Beginner's Guide**

## **1. What is Spring Boot?**
Spring Boot is an **opinionated framework** built on top of the Spring framework that simplifies:
- **Configuration** (auto-configuration)
- **Dependency management** (starter POMs)
- **Standalone applications** (embedded servers)
- **Production-ready features** (metrics, health checks)

### **Key Advantages:**
🚀 **Rapid development** with sensible defaults  
⚡ **Auto-configuration** eliminates boilerplate  
📦 **Embedded servers** (Tomcat/Jetty) - no WAR deployment needed  
🔌 **Starter dependencies** - one-line library integrations  

## **2. Core Concepts**

### **A. Spring Boot Starters**
Predefined dependency bundles for common use cases:
```xml
<!-- web applications -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- database access -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

### **B. Auto-Configuration**
Spring Boot automatically configures your application based on:
- Present classes in classpath
- Defined beans
- Property settings

*Example*: Just adding `spring-boot-starter-data-jpa` and H2 driver automatically sets up:
- DataSource
- EntityManager
- TransactionManager

### **C. Main Application Class**
```java
@SpringBootApplication // = @Configuration + @EnableAutoConfiguration + @ComponentScan
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args); // Starts embedded server
    }
}
```

## **3. Building a REST API (Step-by-Step)**

### **Step 1: Create Controller**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping
    public List<User> getUsers() {
        return userService.getAllUsers();
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
}
```

### **Step 2: Service Layer**
```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository repository;
    
    public List<User> getAllUsers() {
        return repository.findAll();
    }
}
```

### **Step 3: Repository (JPA)**
```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Auto-implemented CRUD methods
}
```

### **Step 4: Entity Class**
```java
@Entity
public class User {
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private String email;
    // getters/setters
}
```

## **4. Configuration**

### **application.properties**
```properties
# Server
server.port=8081

# Database
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.h2.console.enabled=true

# JPA
spring.jpa.show-sql=true
```

### **YAML Alternative (application.yml)**
```yaml
server:
  port: 8081
  
spring:
  datasource:
    url: jdbc:h2:mem:testdb
  h2:
    console:
      enabled: true
```

## **5. Spring Boot Actuator**
Production-ready features for monitoring:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Endpoints** (accessed via `/actuator`):
- `/health` - Application health
- `/metrics` - Performance metrics
- `/info` - Custom application info

## **6. Common Annotations Cheat Sheet**

| Annotation | Purpose |
|------------|---------|
| `@SpringBootApplication` | Main app configuration |
| `@RestController` | REST controller |
| `@RequestMapping` | URL mapping |
| `@Autowired` | Dependency injection |
| `@Service` | Business logic layer |
| `@Repository` | Data access layer |
| `@Entity` | JPA entity |
| `@Configuration` | Bean definitions |
| `@Value` | Inject property values |

## **7. Running the Application**
```bash
# 1. Using Maven
mvn spring-boot:run

# 2. Running the JAR
java -jar target/myapp.jar
```

## **8. Testing**
```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerTests {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldReturnUsers() throws Exception {
        mockMvc.perform(get("/api/users"))
               .andExpect(status().isOk());
    }
}
```

## **9. Spring Boot vs Traditional Spring**

| Feature | Spring Boot | Traditional Spring |
|---------|------------|--------------------|
| Configuration | Auto | Manual XML/JavaConfig |
| Server | Embedded | External (Tomcat etc.) |
| Dependencies | Starters | Manual management |
| Deployment | Executable JAR | WAR to container |
| Bootstrapping | Quick | More setup required |

## **10. Next Steps to Learn**
1. **Spring Data JPA** - Advanced repository patterns
2. **Spring Security** - Authentication/authorization
3. **Spring Cloud** - Microservices architecture
4. **Testing** - MockMvc, Testcontainers
5. **Docker Integration** - Containerization

Spring Boot dramatically reduces the complexity of Spring applications while maintaining all its power! 🚀

## **Maven - Short Notes for Entry-Level SWEs**

### **What is Maven?**
- **Build automation tool** for Java projects
- Manages **dependencies** (libraries your project needs)
- Standardizes **project structure**
- Uses **XML configuration** (`pom.xml`)
- Handles **compilation, testing, packaging, deployment**

### **Key Features:**
✔ Dependency management  
✔ Standard project layout  
✔ Build lifecycle management  
✔ Plugin ecosystem  

---

## **Maven Build Lifecycle Stages**

Maven has **3 built-in lifecycles** with **phases** executed in order:

### **1. Default Lifecycle** (Main Build Process)
| Phase | Description |
|-------|-------------|
| `validate` | Checks project correctness |
| `compile` | Compiles source code |
| `test` | Runs unit tests |
| `package` | Creates JAR/WAR file |
| `verify` | Runs integration tests |
| `install` | Installs package to local Maven repo |
| `deploy` | Copies package to remote repository |

### **2. Clean Lifecycle**
| Phase | Description |
|-------|-------------|
| `pre-clean` | Pre-clean tasks |
| `clean` | Removes `target/` directory |
| `post-clean` | Post-clean tasks |

### **3. Site Lifecycle** (Documentation)
| Phase | Description |
|-------|-------------|
| `pre-site` | Pre-site tasks |
| `site` | Generates project documentation |
| `post-site` | Post-site tasks |
| `site-deploy` | Deploys site to server |

---

### **How to Use?**
```bash
# Common commands (executes all phases UP TO specified phase)
mvn clean       # Runs clean lifecycle
mvn compile     # Compiles code
mvn test        # Runs tests
mvn package     # Creates JAR/WAR
mvn install     # Installs to local repo

# Combined commands
mvn clean package  # Cleans then builds
mvn clean test     # Cleans then runs tests
```

### **Key Files**
- `pom.xml` - Project configuration
- `~/.m2/settings.xml` - User-specific Maven settings

Maven makes Java project management consistent and dependency handling effortless! 🛠️
