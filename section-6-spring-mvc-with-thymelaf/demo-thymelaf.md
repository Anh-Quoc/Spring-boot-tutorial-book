# Demo Thymelaf



{% code title="DemoController.java" lineNumbers="true" %}
```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class DemoController {
    
    @GetMapping("/index")
    public String homePage(Model theModel) {

        theModel.addAttribute("theDate", new java.util.Date());

        return "index";
    }
}
```
{% endcode %}



{% code title="index.html" lineNumbers="true" %}
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">

<head>
    <title>Thymeleaf Demo</title>
</head>

<body>

    <p th:text="'Time on the server is ' + ${theDate}" />

</body>

</html>
```
{% endcode %}
