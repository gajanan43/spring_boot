# Questions:

1. How does Spring Boot auto-configuration work internally?
2. What happens during application startup lifecycle?
3. Difference between @ComponentScan and @EnableAutoConfiguration?
4. How does BeanFactory differ from ApplicationContext?
5. What is circular dependency and how to resolve it?
6. How does Spring manage thread safety for singleton beans?
7. What is proxy in Spring AOP?
8. JDK Dynamic Proxy vs CGLIB?
9. How does @Transactional work internally?
10. What is transaction propagation and isolation?
11. How to handle distributed transactions?
12. What is saga pattern?
13. How does Hibernate first-level and second-level cache work?
14. What is dirty checking in JPA?
15. What is N+1 problem and how to fix it?
16. How does pagination work at DB level?
17. What is optimistic vs pessimistic locking?
18. How do you design high availability systems?
19. What is load balancing at application level?
20. How does API Gateway work?
21. How does OAuth2 flow work internally?
22. What is JWT token lifecycle?
23. How to prevent SQL Injection and XSS?
24. What is CORS and how browser enforces it?
25. How does Spring Security filter chain work?
26. What is idempotency in REST?
27. How to design versioned APIs?
28. What is eventual consistency?
29. What is CAP theorem?
30. How does Kafka ensure message durability?
31. What is consumer group?
32. What is exactly-once processing?
33. How does WebClient differ from RestTemplate?
34. What is backpressure in reactive streams?
35. Mono vs Flux use cases?
36. How does non-blocking IO improve scalability?
37. How does Kubernetes manage pod scaling?
38. What is HPA?
39. What is service mesh?
40. How does circuit breaker work internally?
41. What is bulkhead pattern?
42. How does Redis caching strategy work?
43. Cache eviction policies?
44. What is TTL?
45. How do you debug memory leaks?
46. How to analyze heap dump?
47. How to trace slow APIs in production?
48. What is distributed tracing?
49. What is blue-green vs canary deployment?
50. How do you handle zero-downtime deployments?

## 1. What is Spring Boot?

**Spring Boot is a framework built on top of Spring** that helps you create Spring applications **quickly with minimum configuration**.

👉 It removes boilerplate code and XML configuration.

**Example:**
Instead of configuring server, beans, and XML manually, Spring Boot lets you run the app using:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

## 2. Why is Spring Boot used?

Spring Boot is used because:

* Less configuration
* Embedded server (no Tomcat install needed)
* Faster development
* Production-ready features (Actuator)

**Interview line:**

> Spring Boot simplifies Spring application development by providing auto-configuration, embedded servers, and starter dependencies.

---

## 3. Difference between Spring and Spring Boot

| Spring                    | Spring Boot        |
| ------------------------- | ------------------ |
| Needs XML / manual config | Auto configuration |
| External server required  | Embedded server    |
| More setup                | Minimal setup      |
| Slower to start           | Faster             |

---

## 4. What problems does Spring Boot solve?

Spring Boot solves:

* Too much XML configuration
* Manual dependency management
* External server configuration
* Slow project setup

---

## 5. What is auto-configuration in Spring Boot?

**Auto-configuration automatically configures beans** based on:

* Dependencies in classpath
* application.properties settings

**Example:**
If `spring-boot-starter-web` is present → Tomcat + DispatcherServlet are auto-configured.

---

## 6. How does Spring Boot auto-configuration work?

It uses:

* `@EnableAutoConfiguration`
* `spring.factories`
* Conditional annotations like:

  * `@ConditionalOnClass`
  * `@ConditionalOnMissingBean`

**Interview line:**

> Spring Boot checks classpath and conditions, then creates beans automatically.

---

## 7. What is Spring Boot Starter?

Starters are **predefined dependency bundles**.

**Example:**

```xml
spring-boot-starter-web
```

Includes:

* Spring MVC
* Tomcat
* Jackson

---

## 8. Difference between @Component, @Service, and @Repository

| Annotation  | Use                               |
| ----------- | --------------------------------- |
| @Component  | Generic bean                      |
| @Service    | Business logic                    |
| @Repository | DAO layer + exception translation |

👉 Functionally same, but used for **layer clarity**.

---

## 9. What is @SpringBootApplication?

It is the **main annotation** for Spring Boot apps.

**Example:**

```java
@SpringBootApplication
public class App { }
```

---

## 10. What annotations are included inside @SpringBootApplication?

It contains:

* `@Configuration`
* `@EnableAutoConfiguration`
* `@ComponentScan`

---

## 11. What is dependency injection?

**Dependency Injection (DI)** means **Spring creates and injects objects automatically**.

**Example:**

```java
@Service
class EmployeeService { }

@RestController
class EmployeeController {
    @Autowired
    EmployeeService service;
}
```

---

## 12. Difference between @Autowired and constructor injection

| @Autowired      | Constructor Injection |
| --------------- | --------------------- |
| Field-based     | Constructor-based     |
| Less testable   | More testable         |
| Not recommended | Recommended           |

**Preferred:**

```java
public EmployeeController(EmployeeService service) {
    this.service = service;
}
```

---

## 13. What is Inversion of Control (IoC)?

IoC means **Spring controls object creation**, not the developer.

👉 You don’t use `new`, Spring does it.

---

## 14. What is application.properties file?

Used to **configure application settings**.

**Example:**

```properties
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
```

---

## 15. Difference between application.properties and application.yml

| properties | yml               |
| ---------- | ----------------- |
| Key=value  | Indentation based |
| Simple     | Clean & readable  |

**YML example:**

```yml
server:
  port: 8081
```

---

## 16. How do you change the default port in Spring Boot?

```properties
server.port=9090
```

---

## 17. What is embedded server in Spring Boot?

Spring Boot includes server **inside the application**.

👉 No need to deploy WAR file.

---

## 18. Which embedded servers are supported?

* Tomcat (default)
* Jetty
* Undertow

---

## 19. What is REST API?

REST API allows **communication between client and server using HTTP methods**.

Methods:

* GET
* POST
* PUT
* DELETE

---

## 20. Difference between @Controller and @RestController

| @Controller  | @RestController |
| ------------ | --------------- |
| Returns view | Returns JSON    |
| Used for MVC | Used for REST   |

---

## 21. Difference between @RequestMapping and @GetMapping

| @RequestMapping      | @GetMapping |
| -------------------- | ----------- |
| Supports all methods | Only GET    |
| Generic              | Specific    |

---

## 22. What is @PathVariable?

Used to read values from URL.

**Example:**

```java
@GetMapping("/emp/{id}")
public Employee get(@PathVariable int id) { }
```

---

## 23. What is @RequestParam?

Reads query parameters.

**Example:**

```java
@GetMapping("/emp")
public Employee get(@RequestParam int id) { }
```

---

## 24. Difference between PUT and POST

| POST           | PUT        |
| -------------- | ---------- |
| Create         | Update     |
| Not idempotent | Idempotent |

---

## 25. What is @RequestBody?

Used to convert **JSON → Java object**.

```java
@PostMapping("/save")
public Employee save(@RequestBody Employee emp) { }
```

---

## 26. What is Spring Data JPA?

It simplifies database operations using **JPA repositories**.

👉 No need to write SQL.

---

## 27. What is CrudRepository?

Provides basic CRUD operations.

Methods:

* save()
* findById()
* deleteById()

---

## 28. Difference between CrudRepository and JpaRepository

| CrudRepository | JpaRepository        |
| -------------- | -------------------- |
| Basic CRUD     | Advanced             |
| No pagination  | Pagination + sorting |

---

## 29. What is Hibernate?

Hibernate is an **ORM framework** that maps Java objects to DB tables.

---

## 30. What is ORM?

ORM = **Object Relational Mapping**

👉 Java class ↔ Database table

---

## 31. Difference between Comparable and Comparator

| Comparable   | Comparator     |
| ------------ | -------------- |
| Single logic | Multiple logic |
| compareTo()  | compare()      |

---

## 32. What is CORS in Spring Boot?

CORS allows **frontend and backend from different origins** to communicate.

```java
@CrossOrigin
```

---

## 33. What is exception handling in Spring Boot?

Handling runtime errors gracefully using:

* try-catch
* @ExceptionHandler
* @ControllerAdvice

---

## 34. What is pagination and sorting?

Used to **fetch limited data**.

```java
PageRequest.of(page, size, Sort.by("name"))
```

---

## 35. Difference between JPA and Hibernate

| JPA           | Hibernate      |
| ------------- | -------------- |
| Specification | Implementation |
| Interface     | ORM tool       |

---

## 36. What is JpaRepository?

Advanced repository with:

* Pagination
* Sorting
* Batch operations

---

## 37. What is Spring Data JPA?

👉 Same as #26
It reduces boilerplate code for DB access.

---

## 38. What is Optional class?

Used to **avoid NullPointerException**.

```java
Optional<Employee> emp = repo.findById(id);
```

---

## 39. What is default method in interface?

Interface method with implementation (Java 8+).

```java
default void show() {
    System.out.println("Hello");
}
```

---

## 40. What is Spring Boot Actuator?

Provides **monitoring endpoints**.

Examples:

* `/actuator/health`
* `/actuator/metrics`

---

## 41. What is exception handling in Spring Boot?

Handled using:

* @ExceptionHandler
* @ControllerAdvice
* Custom exceptions

---

## 42. What is @ControllerAdvice?

Global exception handling.

```java
@ControllerAdvice
public class GlobalExceptionHandler { }
```

---

## 43. What is Spring Boot Security?

Used to **secure applications**:

* Authentication
* Authorization

---

## 44. What is JWT authentication?

JWT = JSON Web Token
Used for **stateless authentication**.

Flow:

1. Login
2. Server generates token
3. Client sends token in headers
4. Server validates token

---


