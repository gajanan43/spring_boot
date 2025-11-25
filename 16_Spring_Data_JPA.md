# Spring Data JPA:

1) What is ORM & JPA
2) Creating Table & Inserting Data
3) Findall
4) FindById
5) JPQL Query
6) Update & Delete
7) JPA in Job App
8) Loading Data & Entity
9) Search By Keyword
    
## 2) 

```


@SpringBootApplication
public class SpringDataJpaApplication {

	public static void main(String[] args) {
	    ApplicationContext context= SpringApplication.run(SpringDataJpaApplication.class, args);

        StudentRepo repo=context.getBean(StudentRepo.class);

        Student s1=context.getBean(Student.class);
        Student s2=context.getBean(Student.class);
        Student s3=context.getBean(Student.class);

        s1.setId(1);
        s1.setFirstName("Gajanan");
        s1.setAge(22);

        s2.setId(2);
        s2.setFirstName("Virat");
        s2.setAge(38);

        s3.setId(3);
        s3.setFirstName("Rohit");
        s3.setAge(39);

        repo.save(s1);

	}

}




@Component
@Entity
public class Student {
    @Id
    private int id;
    private String firstName;
    private int age;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", firstName='" + firstName + '\'' +
                ", age='" + age + '\'' +
                '}';
    }
}




@Repository
public interface StudentRepo extends JpaRepository<Student,Integer> {

}



spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=mysql
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql= true

```

## 3) Findall

```

```
