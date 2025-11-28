# JWT(Json Web Toekn) & OAuth2:
1) Encryption & Decryption
2) Digital Signature
3) Why JWT
4) What is JWT
5) Project Setup for JWT
6) Custom Login
7) Generating Token
8) Token Generated
9) Creating a JWT filter
10) Setting AuthToken in SecurityContext
11) Validating Token
12) JWT Summary

    
## 13) Implementing OAuth2
```
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests(auth-> auth
                .anyRequest().authenticated()).oauth2Login(Customizer.withDefaults());

        return http.build();
    }
}

```
 
## 14) Google OAuth2 Login(Login through Google)
```

spring.security.oauth2.client.registration.google.client-id=YOUR_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_CLIENT_SECRET
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.google.scope=openid,profile,email
```
    
## 15) Github Login(Login through GithHub)
```
spring.application.name=SpringOAuth

spring.security.oauth2.client.registration.github.client-id=YOUR_CLIENT_ID
spring.security.oauth2.client.registration.github.client-secret=YOUR_CLIENT_SECRET
spring.security.oauth2.client.registration.github.scope=user:email

spring.security.oauth2.client.registration.github.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.provider.github.authorization-uri=https://github.com/login/oauth/authorize
spring.security.oauth2.client.provider.github.token-uri=https://github.com/login/oauth/access_token
spring.security.oauth2.client.provider.github.user-info-uri=https://api.github.com/user

```
