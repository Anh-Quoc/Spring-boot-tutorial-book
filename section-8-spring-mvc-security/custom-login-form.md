# Custom login form



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

        http.authorizeHttpRequests(configurer -> {
            configurer.anyRequest().authenticated();
        });
        
        http.formLogin(form ->
        {
            form.loginPage("/showMyLoginPage")
                    .loginProcessingUrl("/authenticateTheUser")
                    .permitAll();
        });
        return http.build();
    }
}

```
{% endcode %}



{% code title="LoginController.java" lineNumbers="true" %}
```java
public class LoginController {
    @GetMapping("/showMyLoginPage")
    public String showMyLoginPage() {
        return "plain-login";
    }
}
```
{% endcode %}



{% code title="plain-login.html" lineNumbers="true" %}
```html
<form action="#" th:action="@{/authenticateTheUser}" method="POST">
    <table>
        <tr>
            <td>User name:</td>
            <td><label>
                <input type="text" name="username"/>
            </label></td>
        </tr>
        <tr>
            <td>Password:</td>
            <td><label>
                <input type="password" name="password"/>
            </label></td>
        </tr>
        <tr>
            <td>
                <div th:if="${param.error}">
                    <i>Sorry! You entered invalid username or password</i>
                </div>
            </td>
        </tr>
        <tr>
            <td></td>
            <td><input type="submit" value="Login"/></td>
        </tr>
    </table>
</form>

<!-- Check if user logout -->
<div th:if="${param.logout}">
    <div class="alert alert-success col-xs-offset-1 col-xs-10">
        You have been logged out successfully.
    </div>
</div>

<!-- Check if wrong username or password -->
<div th:if="${param.error}">
    <div class="alert alert-danger col-xs">
        Sorry! You entered invalid username or password
    </div>
</div>

```
{% endcode %}
