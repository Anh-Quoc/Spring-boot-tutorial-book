---
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# Section 3: Database Access with Hibernate/JPA CRUD

<pre class="language-markup" data-title="application.properties"><code class="lang-markup">spring.datasource.url=jdbc:mysql://localhost:3306/student_tracker
spring.datasource.username=springstudent
spring.datasource.password=springstudent

# Turn off the Spring Boot banner
<strong># spring.main.banner-mode=off
</strong>
# Reduce logging level. Set logging level to warn
# logging.level.root=warna

# Add logging configs to display SQL statements
logging.level.org.hibernate.SQL=debug
logging.level.org.hibernate.orm.jdbc.bind=trace

# Configure JPA/Hibernate to auto create the tables
# the "update" config will keep existing data in the table
spring.jpa.hibernate.ddl-auto=update
</code></pre>

{% code title="porm.xml" %}
```xml
<dependency>			
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
	<groupId>com.mysql</groupId>
	<artifactId>mysql-connector-j</artifactId>
	<scope>runtime</scope>
</dependency>

```
{% endcode %}
