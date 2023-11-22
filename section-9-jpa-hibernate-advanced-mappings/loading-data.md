# Loading Data

## Eager Loading

Will load all dependent entities. Not perfer



## Lazy Loading

Only load data when absolutely needed





| Mapping         | Default Fetch Type |
| --------------- | ------------------ |
| **@OneToOne**   | FetchType.EAGER    |
| **@OneToMany**  | FetchType.LAZY     |
| **@ManyToOne**  | FetchType.EAGER    |
| **@ManyToMany** | FetchType.LAZY     |

To overriding Default Fetch Type:

```java
@OneToMany( fetch = FetchType.EAGER,
            mappedBy = "instructor",
            cascade = {
                        CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH
            })
private List<Course> courses;
```

If the Hibernate session is closed and attempt to retrieve lazy data -> Hibernate will throw an exception.

When retrieve a filed List\<Course> LAZY in Instructor

{% code title="AppDAOImpl.java" lineNumbers="true" %}
```java
    @Override
    public List<Course> findCoursesByInstructorId(int id) {
        TypedQuery<Course> query = entityManager.createQuery("from Course where instructor.id = :data", Course.class);
        query.setParameter("data", id);
        List<Course> courses = query.getResultList();
        return courses;
    }
```
{% endcode %}



<pre class="language-java" data-line-numbers><code class="lang-java">private void findCoursesByInstructorId(AppDAO appDAO) {
    int id = 1;
    Instructor instructor = appDAO.findInstructorById(id);
    List&#x3C;Course> listCourse = appDAO.findCoursesByInstructorId(id);
    
    instructor.setCourses(listCourse);
    
<strong>    System.out.println("the associated courses: " + instructor.getCourses());
</strong>}
</code></pre>
