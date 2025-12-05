# Annotations


## 1. **@Configuration**

➡ **This class contains bean-creating methods.**

Example:

```java
@Configuration
class AppConfig {}
```

---

## 2. **@Bean**

➡ **Create this object and put it in Spring container.**

Example:

```java
@Bean
public Laptop laptop() {
    return new Laptop();
}
```

---

## 3. **@Component**

➡ **Spring, please create an object of this class automatically.**

Example:

```java
@Component
class Laptop {}
```

---

## 4. **@ComponentScan**

➡ **Spring, look in this package and find all @Component classes.**

Example:

```java
@ComponentScan("com.example")
```

---

## 5. **@Autowired**

➡ **Spring, give me the object automatically (inject it).**

Example:

```java
@Autowired
Laptop laptop;
```

---

# 🌱 **Spring Stereotype Annotations**

(They all mean: special type of @Component)

## 6. **@Service**

➡ Class that contains business logic.
(Simple: **Service class**)

## 7. **@Repository**

➡ Class that handles database operations.
(Simple: **DAO class**)

## 8. **@Controller**

➡ Class that handles web requests in Spring MVC.
(Simple: **Web controller**)

## 9. **@RestController**

➡ Same as @Controller + automatically returns JSON.
(Simple: **API controller**)

---

# 🌱 **Injection Annotations**

## 10. **@Qualifier**

➡ When there are two beans of same type, choose one.

```java
@Autowired
@Qualifier("laptop1")
Laptop laptop;
```

---

## 11. **@Value**

➡ Inject simple values (string, number, etc.)

```java
@Value("25")
int age;
```

---

# 🌱 **Scope Annotation**

## 12. **@Scope**

➡ Defines bean scope (singleton, prototype).

```java
@Scope("prototype")
```

---

# 🌱 **Lifecycle Annotations**

## 13. **@PostConstruct**

➡ Run method **after** object is created.

## 14. **@PreDestroy**

➡ Run method **before** object is destroyed.

---

# 🌱 **Spring Boot Annotations**

## 15. **@SpringBootApplication**

➡ Combines:

* @Configuration
* @EnableAutoConfiguration
* @ComponentScan

Simple meaning: **Start Spring Boot application with auto config.**

---

# 🌟 SUPER SIMPLE SUMMARY

| Annotation                   | Meaning                          |
| ---------------------------- | -------------------------------- |
| @Component                   | Create object automatically      |
| @Autowired                   | Give me the object automatically |
| @Configuration               | Class that creates beans         |
| @Bean                        | Create this specific bean        |
| @ComponentScan               | Where to search for components   |
| @Service                     | Business logic class             |
| @Repository                  | Database class                   |
| @Controller                  | Web handler                      |
| @RestController              | API handler                      |
| @Value                       | Insert simple value              |
| @Qualifier                   | Choose correct bean              |
| @Scope                       | Bean scope                       |
| @PostConstruct / @PreDestroy | Lifecycle methods                |
| @SpringBootApplication       | Start Spring Boot app            |

