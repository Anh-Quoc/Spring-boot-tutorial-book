# User and Role Information



{% code title="index.html" %}
```html
<html lang="en" xmlns:th="http://www.thymeleaf.org"
                xmlns:sec="http://www.thymeleaf.org/extras/spring-security">

<body>

    <p>
        User: <span sec:authentication="principal.username"></span>
        <br><br>
        Role(s): <span sec:authentication="principal.authorities"></span>
    </p>

</body>
</html>

```
{% endcode %}

