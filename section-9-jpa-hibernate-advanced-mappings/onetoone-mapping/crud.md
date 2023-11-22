# CRUD



<details>

<summary>FIND</summary>

**When retrieves a Instructor, InstructorDetail also retried.**

{% code title="AppDAOImpl.java" lineNumbers="true" %}
```java
    @Override
    public Instructor findInstructorById(int id) {
        return entityManager.find(Instructor.class, id);
    }
```
{% endcode %}

</details>

<details>

<summary>DELETE</summary>

When delete a Instructor, InstructorDetail also being deleted

{% code title="AppDAOImpl.java" lineNumbers="true" %}
```java
    @Override
    @Transactional
    public void deleteInstructorById(int id) {
        Instructor instructor = findInstructorById(id);
        entityManager.remove(instructor);
    }
```
{% endcode %}

</details>
