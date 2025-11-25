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
    
## 2) Creating Table & Inserting Data

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
StudentRepo repo=context.getBean(StudentRepo.class);
System.out.println(repo.findAll());
```

## 4) FindById

```
System.out.println(repo.findById(1));

OR

Optional<Student> student=repo.findById(1);
System.out.println(student.orElse(new Student()));
```

## 5) JPQL Query

```
@Query("select s from Student s where s.firstName=?1") //doesn't mandatory to write 
List<Student> findByName(String name);

System.out.println(repo.findByName("Virat"));

OR

List<Student> findByAgeGreaterThan(int age);
System.out.println(repo.findByAgeGreaterThan(20));
```

## 6) Update & Delete

```
1 Update

s2.setId(2);
s2.setFirstName("Kohli"); //Change name
s2.setAge(39); //change age

repo.save(s2);  // if not in table then insert data otherwise work as UPDATE query

2 Delete

repo.delete(s2);
```

## 7) JPA in Job App



