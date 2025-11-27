# Spring Security:

## 1) OWASP Top 10: Open Web Application Security Project

| Rank    | Category (2025)                           |                                                                                                                                                                   
| ------- | ----------------------------------------- | 
| **A01** | **Broken Access Control**                 | 
| **A02** | **Security Misconfiguration**             |
| **A03** | **Software Supply Chain Failures**        |
| **A04** | **Cryptographic Failures**                | 
| **A05** | **Injection**                             | 
| **A06** | **Insecure Design**                       | 
| **A07** | **Authentication Failures**               | 
| **A08** | **Software or Data Integrity Failures**   |
| **A09** | **Logging & Alerting Failures**           |
| **A10** | **Mishandling of Exceptional Conditions** |


## 2) Creating a Spring Security Project

- Add Dependencies Spring Security,Spring Web, Spring Boot DevTools.

## 3) Default Login Form:

- UserName is ```user & password ```generated whenever run project

## 4) Spring Security Filters:

## 5) Session ID:

```
    @GetMapping("/hello")
    public String Hello(HttpServletRequest request) {
        return "Hello World" + request.getSession().getId();
    }
```
- If you logout then new session created
- Whenever i login into website new session ID will be created
  
## 6) Setting Username & Password:

```
application.properties

spring.security.user.name=user
spring.security.user.password=1234
```

## 7) Basic Auth Using Postman:

- By using Postman is not working but On Postman Authorization set AS Basic Auth & there set username & password.

## 8) What is CSRF(Cross-Site Request Forgery):

## 9) Error without CSRF Token:

```
    @PostMapping("/students")
    public void getStudent(@RequestBody Student student){
        students.add(student);
    }
```

- It cann't work beacuse the CSRF is blocked

## 10) Sending CSRF Token:

```
    @GetMapping("csrf-token")
    public CsrfToken getCsrfToken(HttpServletRequest request) {
        return (CsrfToken) request.getAttribute("_csrf");
    }
```
- By using above mapping will get a CSRF Token use it for POST Mapping.
- Every time will get this id to POST Mapping.

## 11) Same Site Strict:

```
application.properties

server.servlet.session.cookie.same-site=strict
```

## 12) Security Configuration:

```
config-package

@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http.build();
    }
}
```
- It is allow to write our own security.

## 13) Disabling CSRF Token:

```
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf(customizer-> customizer.disable());
        http.authorizeHttpRequests(requests -> requests.anyRequest().authenticated());
        http.httpBasic(Customizer.withDefaults());
        http.sessionManagement(session-> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));

        return http.build();
    }
```

## 14) Without Lamda:

## 15) Getting Ready for User Databases

## 16) Working With Multiple Users

```
    @Bean
    public UserDetailsService  userDetailsService() {

        UserDetails user=  User
                .withDefaultPasswordEncoder()
                .username("user")
                .password("user@123")
                .roles("USER")
                .build();

        UserDetails admin=  User
                .withDefaultPasswordEncoder()
                .username("admin")
                .password("admin@123")
                .roles("ADMIN")
                .build();

        return new InMemoryUserDetailsManager(user,admin);
    }

```

## 17) Creating User Table & db Properties:

```

```
