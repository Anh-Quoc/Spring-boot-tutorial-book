# Logout form





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
        
//      ...
        http.logout(logout ->
                {
                    logout.logoutSuccessUrl("/loginPage").permitAll();

                });
//       ...
                
        return http.build();
    }
}

```
{% endcode %}

{% code title="index.html" %}
```html
<form action="#" th:action="@{/logout}" method="POST">
    <input type="submit" value="Logout" />
</form>
```
{% endcode %}



