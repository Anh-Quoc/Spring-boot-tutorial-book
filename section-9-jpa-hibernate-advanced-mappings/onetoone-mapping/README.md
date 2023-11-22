# @OneToOne Mapping

SQL Code

## Using SQL to create OneToOne Mapping

```sql
DROP SCHEMA IF EXISTS `hb-01-one-to-one-uni`;

CREATE SCHEMA `hb-01-one-to-one-uni`;

use `hb-01-one-to-one-uni`;

SET FOREIGN_KEY_CHECKS = 0;

CREATE TABLE `instructor_detail` (
  `id` int NOT NULL AUTO_INCREMENT,
  `youtube_channel` varchar(128) DEFAULT NULL,
  `hobby` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;


CREATE TABLE `instructor` (
  `id` int NOT NULL AUTO_INCREMENT,
  `first_name` varchar(45) DEFAULT NULL,
  `last_name` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `instructor_detail_id` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_DETAIL_idx` (`instructor_detail_id`),
  
  CONSTRAINT `FK_DETAIL` FOREIGN KEY (`instructor_detail_id`) REFERENCES `instructor_detail` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
  
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=latin1;

SET FOREIGN_KEY_CHECKS = 1;
```

Cascade Types

| Cascade Type | Description                                                                                                                                                                           |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PERSIST**  | If entity is <mark style="background-color:green;">persist/saved</mark>, related entity will also be <mark style="background-color:green;">persisted</mark>                           |
| **REMOVE**   | If  entity is <mark style="background-color:green;">removed/deleted</mark>, related entity will also be <mark style="background-color:green;">deleted</mark>                          |
| **REFRESH**  | If entity is <mark style="background-color:green;">refreshed</mark>, related entity will also be <mark style="background-color:green;">refreshed</mark>                               |
| **DETACH**   | If entity is <mark style="background-color:green;">detached</mark> (not associated w/section), then related entity will also be <mark style="background-color:green;">detached</mark> |
| **MERGE**    | If entity is <mark style="background-color:green;">merged</mark>, then related entity will also be <mark style="background-color:green;">merged</mark>                                |
| **ALL**      | All of above cascade types                                                                                                                                                            |

> By default, no operations are cascaded. It have to be declared.
>
> Ex in java code: @OneToOne(cascade=CascadeType.All)

## Using Java Hibernate to create OneToOne Mapping

{% code title="Instructor.java" lineNumbers="true" fullWidth="false" %}
```java
import jakarta.persistence.*;

@Entity
@Table(name = "instructor")
public class Instructor {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    @Column(name = "email")
    private String email;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "instructor_detail_id")
    private InstructorDetail instructorDetail;

    public Instructor() {
    }

    public Instructor(String firstName, String lastName, String email) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
    }
    // Getter and Setter
    
    // toString()
}

```
{% endcode %}



{% code title="InstructorDetail.java" lineNumbers="true" %}
```java
import jakarta.persistence.*;

@Entity
@Table(name = "instructor_detail")
public class InstructorDetail {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;

    @Column(name = "youtube_channel")
    private String youtubeChannel;

    @Column(name = "hobby")
    private String hobby;

    public InstructorDetail() {
    }

    public InstructorDetail(String youtubeChannel, String hobby) {
        this.youtubeChannel = youtubeChannel;
        this.hobby = hobby;
    }
    // Getter and Setter
    
    // toString()
}

```
{% endcode %}

**1 Instructor to 1 InstructorDetail. In order to create Instructor, InstructorDetail need to be create first.**



## Bi-Directional Association

<pre class="language-java" data-title="InstructorDetail.java" data-line-numbers><code class="lang-java">    @OneToOne(mappedBy = "<a data-footnote-ref href="#user-content-fn-1">instructorDetail</a>", cascade = CascadeType.ALL)
    private Instructor instructor;
    
    public Instructor getInstructor() {
        return instructor;
    }

    public void setInstructor(Instructor instructor) {
        this.instructor = instructor;
    }
</code></pre>

Incase you need to delete a instructorDetail but **keep** instructor.\
Before delete a instructorDetail, you need to set this field instructorDetail[^2] in Instructor.java to null

{% code title="InstructorDetail.java" lineNumbers="true" %}
```java
@OneToOne(mappedBy = "instructorDetail",
            cascade = {CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH})
private Instructor instructor;
```
{% endcode %}

[^1]: Refers to "instructorDetail" property

    in "Instructor" class

    ```java
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "instructor_detail_id")
    private InstructorDetail instructorDetail;
    ```

[^2]: {% code title="Instructor.java" %}
    ```java
    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "instructor_detail_id")
    private InstructorDetail instructorDetail;
    ```
    {% endcode %}
