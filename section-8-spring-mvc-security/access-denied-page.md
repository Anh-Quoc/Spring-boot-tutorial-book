# Access Denied Page



{% code title="SecurityConfig.java" lineNumbers="true" %}
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        
        http.exceptionHandling(configurer ->
                {
                    configurer.accessDeniedPage("/access-denied");
                });
                
        return http.build();
    }
}

```
{% endcode %}



{% code title="Controller.java" %}
```java
@GetMapping("/access-denied")
public String showAccessDenied() {
    return "access-denied-page";
}
```
{% endcode %}



