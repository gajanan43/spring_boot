Here is your **Spring Security topic re-written in a clean, proper, simple and easy-to-remember format**, keeping ALL your points in order 👇👇

---

# 🌿 **SPRING SECURITY — SIMPLE NOTES**

---

# ## **1️⃣ OWASP Top 10 (2025)**

OWASP = **Open Web Application Security Project**
Provides top security risks for web applications.

| Code | Category                              |
| ---- | ------------------------------------- |
| A01  | Broken Access Control                 |
| A02  | Security Misconfiguration             |
| A03  | Software Supply Chain Failures        |
| A04  | Cryptographic Failures                |
| A05  | Injection                             |
| A06  | Insecure Design                       |
| A07  | Authentication Failures               |
| A08  | Software or Data Integrity Failures   |
| A09  | Logging & Alerting Failures           |
| A10  | Mishandling of Exceptional Conditions |

👉 **These are common security vulnerabilities every developer should know.**

---

# ## **2️⃣ Creating Spring Security Project**

Add dependencies:

* **Spring Web**
* **Spring Security**
* **Spring Boot DevTools**
* (Optional: Spring Data JPA + DB)

---

# ## **3️⃣ Default Login Form**

When you run a Spring Security app:

* Default login page is auto created
* Default username = `user`
* Password = printed in console

```
Using generated security password: a87hdjsj8sjhd…
```

---

# ## **4️⃣ Spring Security Filters**

Spring Security uses a **filter chain** to handle:

* Authentication
* Authorization
* CSRF validation
* Session creation
* Exception handling

You don’t manually manage security filters.

---

# ## **5️⃣ SESSION ID**

```java
@GetMapping("/hello")
public String hello(HttpServletRequest request) {
    return "Hello World " + request.getSession().getId();
}
```

📌 Important points:

* When user logs in → new session ID is created
* If user logs out → old session destroyed
* Every login creates **fresh session ID**

---

# ## **6️⃣ Setting Username & Password**

`application.properties`

```properties
spring.security.user.name=user
spring.security.user.password=1234
```

👉 This replaces auto-generated password.

---

# ## **7️⃣ Basic Auth in Postman**

By default, POST and GET only work if authentication is given.

In Postman:

* Authorization tab → **Basic Auth**
* Enter username & password

✔ Then calls will work.

---

# ## **8️⃣ What is CSRF?**

**CSRF → Cross Site Request Forgery**

Attack where someone tricks your browser to send unwanted requests to your application.

Spring Security ENABLES CSRF protection for:

* POST
* PUT
* DELETE

(for browser-based apps)

---

# ## **9️⃣ Error Without CSRF Token**

```java
@PostMapping("/students")
public void saveStudent(@RequestBody Student student){
    students.add(student);
}
```

⚠ This fails because:

* POST request is **blocked**
* CSRF token is missing

---

# ## **🔟 Getting CSRF Token**

```java
@GetMapping("/csrf-token")
public CsrfToken getCsrfToken(HttpServletRequest request) {
    return (CsrfToken) request.getAttribute("_csrf");
}
```

📌 Use this token for POST/PUT/DELETE requests.

* You need a fresh token for each request
* Attach it in request headers

---

# ## **1️⃣1️⃣ Same-Site Strict Cookie**

```properties
server.servlet.session.cookie.same-site=strict
```

Ensures browser does not share cookie with external sites.

✔ Improves security
✔ Protects against CSRF

---

# ## **1️⃣2️⃣ Security Configuration**

To allow **custom security rules**, create config:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http.build();
    }
}
```

This enables you to override defaults.

---

# ## **1️⃣3️⃣ Disabling CSRF**

```java
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

    http.csrf(csrf -> csrf.disable());
    http.authorizeHttpRequests(req -> req.anyRequest().authenticated());
    http.httpBasic(Customizer.withDefaults());
    http.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));

    return http.build();
}
```

👉 Very common for **REST APIs**
👉 Stateless (no session)
👉 Basic Auth allowed

---

# ## **1️⃣4️⃣ Without Lambda (Older Style)**

(Just remember: same as above but older syntax)

---

# ## **1️⃣5️⃣ Getting Ready for User DB**

Before storing users in DB:

* Create Users table
* Implement UserDetailsService
* Add AuthenticationProvider

---

# ## **1️⃣6️⃣ Multiple Users In-Memory**

```java
@Bean
public UserDetailsService  userDetailsService() {

    UserDetails user = User
            .withDefaultPasswordEncoder()
            .username("user")
            .password("user@123")
            .roles("USER")
            .build();

    UserDetails admin = User
            .withDefaultPasswordEncoder()
            .username("admin")
            .password("admin@123")
            .roles("ADMIN")
            .build();

    return new InMemoryUserDetailsManager(user,admin);
}
```

✔ For testing only
✔ Not used in production

---

# ## **1️⃣7️⃣ Creating User Table**

Use JPA Entity + Repository.

---

# ## **1️⃣8️⃣ Authentication Provider**

```java
@Bean
public AuthenticationProvider authProvider() {
    DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
    provider.setUserDetailsService(userDetailsService);
    provider.setPasswordEncoder(NoOpPasswordEncoder.getInstance());
    return provider;
}
```

Role:

* Fetch user from DB
* Validate password
* Create authenticated session

---

# ## **1️⃣9️⃣ UserDetails Service (Very Important)**

```java
@Service
public class MyUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepo repo;

    @Override
    public UserDetails loadUserByUsername(String username) {
        return null;  // You will return user details object here
    }
}
```

Spring calls this automatically during login.

---

# ## **2️⃣0️⃣ User Repository**

```java
@Repository
public interface UserRepo extends JpaRepository<User, Integer> {
    User findByUsername(String username);
}
```

✔ Let Spring fetch user from DB

---

# ## **2️⃣1️⃣ UserDetails & Principal**

* `UserDetails` = user object containing

  * username
  * password
  * authorities/roles

* `Principal` = logged-in user info available in controller

---

# ## **2️⃣2️⃣ Summary So Far**

You learned:

* Default login
* CSRF
* Session ID
* Basic Auth
* Custom users
* In-memory auth
* DB auth
* Custom AuthenticationProvider

---

# ## **2️⃣3️⃣ What is BCrypt?**

BCrypt = secure password hashing algorithm

✔ Never store plain text passwords
✔ Store encoded password in DB
✔ BCrypt generates salted hash

---

# ## **2️⃣4️⃣ User Registration API**

```java
@PostMapping("register")
public User register(@RequestBody User user) {
    return service.saveUser(user);
}
```

Service:

```java
public User saveUser(User user) {
    return repo.save(user);
}
```

---

# ## **2️⃣5️⃣ BCrypt Encoding**

```java
private BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(12);

public User saveUser(User user) {
    user.setPassword(encoder.encode(user.getPassword()));
    return repo.save(user);
}
```

✔ DB stores encrypted password
✔ NEVER store plain passwords

---

# ## **2️⃣6️⃣ Setting Password Encoder (IMPORTANT)**

In SecurityConfig:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

And in AuthenticationProvider:

```java
provider.setPasswordEncoder(passwordEncoder());
```

---

# 🌟 **FINAL UNDERSTANDING**

* Spring Security protects web apps by default
* You can fully customize:

  * Users (DB or In-memory)
  * Password encryption (BCrypt)
  * CSRF for browser apps
  * Stateless REST APIs
  * Basic Auth / Session Auth
* SecurityConfig controls everything

---

If you want next, I can teach:

✔ JWT Token Authentication
✔ Role Based Authorization
✔ Request based access rules
✔ Logout behavior
✔ Security for MVC vs REST

Just say **NEXT** 😊
