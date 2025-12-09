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
- Whenever you POST,PUT,DELETE,PATCH api it is not working beacuse it require a csrf token.

## 10) Sending CSRF Token:

```
    @GetMapping("csrf-token")
    public CsrfToken getCsrfToken(HttpServletRequest request) {
        return (CsrfToken) request.getAttribute("_csrf");
    }
```
- By using above mapping will get a CSRF Token use it for POST Mapping.
- Every time will get this id to POST Mapping.

```
http://localhost:8080/api/csrf-token - GET

{
    "headerName": "X-CSRF-TOKEN",
    "parameterName": "_csrf",
    "token": "Token_Value"
}

```
- Add CSRF token inside the headers section of Postman
```
Key: X-CSRF-TOKEN
Valune: Token_Value
```
- Then send the post request at that time is working properly.

## 11) Same Site Strict:

```
application.properties

server.servlet.session.cookie.same-site=strict
```

- By setting this another user cann't access your website.

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
- Spring gives you default security but you can write your own security.
- Most of the time we are working with STATELESS REST Api(it doesn't store session id).
- By using this no need of login form & All.


```
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http){
        http.csrf(customizer->customizer.disable());
        return http.build();
    }
}
```
- Buy writing this we can POST,PUT,DELETE,PATCH methods works properly.

```
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http){
        http.csrf(customizer->customizer.disable());
        http.authorizeHttpRequests(request->
                request.anyRequest().authenticated());

        http.httpBasic(Customizer.withDefaults());
        http.sessionManagement(session->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));

        return http.build();
    }
}

```
- Whenever i load website at time new session id will created it.

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

## 18) Authentication Provider

```
    @Autowired
    private UserDetailsService userDetailsService;

    @Bean
    public AuthenticationProvider authProvider() {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService); // FIXED
        provider.setPasswordEncoder(NoOpPasswordEncoder.getInstance());
        return provider;
    }
```

## 19) Creating a UserDetails Service

```
@Service
public class MyUserDetailsService  implements UserDetailsService {

    @Autowired
    private UserRepo repo;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return null;
    }
}

```

## 20) User Repo

```
@Repository
public interface UserRepo extends JpaRepository<User, Integer> {
    User findByUsername(String username);
}
```
## 21) UserDetails & Principle:

## 22) Summery Till Now

## 23) What is Bcrypt

## 24) User Registration

```
@RestController
public class UserController {

    @Autowired
    private UserService service;

    @PostMapping("register")
    public User register(@RequestBody  User user) {
        return service.saveUser(user);
    }
}

@Service
public class UserService {

    @Autowired
    private UserRepo repo;

    public User saveUser(User user) {
        return repo.save(user);
    }
}

@Repository
public interface UserRepo extends JpaRepository<User, Integer> {
    User findByUsername(String username);
}

```

## 25) Bcrypt Encoding for User Registration

```
  @Autowired
    private UserRepo repo;
    private BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(12);

    public User saveUser(User user) {
        user.setPassword(encoder.encode(user.getPassword()));
        System.out.println(user.getPassword());
        return repo.save(user);
    }
```

## 26) Setting Password Encoder
