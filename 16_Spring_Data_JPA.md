# ## **1️⃣ What is ORM & JPA?**

### **ORM (Object Relational Mapping)**

* ORM means **mapping Java objects to database tables**
* Example:

  * `Student class` → `student table`
  * `firstName variable` → `first_name column`

✔ No need to write SQL manually
✔ JPA/Hibernate generates SQL automatically

---

### **JPA (Java Persistence API)**

* JPA is a **specification** for ORM in Java
* It defines **how to map Java objects to DB**
* JPA itself does **not perform ORM**
* ORM is performed by **JPA implementations like Hibernate**

👉 **JPA = Rules**
👉 **Hibernate = Worker**

---

# ## **2️⃣ Creating Table & Inserting Data**

### **Step 1: Student Entity Class**

```java
@Entity
public class Student {
    @Id
    private int id;
    private String firstName;
    private int age;
}
```

✔ `@Entity` → tells JPA to create a table
✔ `@Id` → primary key

---

### **Step 2: Repository Interface**

```java
@Repository
public interface StudentRepo extends JpaRepository<Student,Integer> {
}
```

✔ `JpaRepository` gives built-in CRUD
✔ No SQL required

---

### **Step 3: Insert Data**

```java
ApplicationContext context = SpringApplication.run(SpringDataJpaApplication.class, args);
StudentRepo repo = context.getBean(StudentRepo.class);

Student s1 = new Student();
s1.setId(1);
s1.setFirstName("Gajanan");
s1.setAge(22);

repo.save(s1);   // INSERT automatically
```

✔ If ID is new → `INSERT`
✔ If ID exists → `UPDATE`

---

### **Step 4: application.properties**

```
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=mysql

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

✔ `ddl-auto=update` makes Hibernate auto-create/modify table
✔ `show-sql=true` prints SQL in console

---

# ## **3️⃣ Find All Records**

```java
System.out.println(repo.findAll());
```

✔ Returns **List<Student>**
✔ Automatically converts DB rows into Student objects

---

# ## **4️⃣ Find By ID**

```java
repo.findById(1)
```

Two ways:

### **Way 1**

```java
System.out.println(repo.findById(1));
```

### **Way 2 (Safer)**

```java
Optional<Student> s = repo.findById(1);
System.out.println(s.orElse(new Student()));
```

✔ `Optional` handles **null** safely

---

# ## **5️⃣ JPQL Queries**

### **Custom query using JPQL**

```java
@Query("select s from Student s where s.firstName=?1")
List<Student> findByName(String name);
```

Use:

```java
System.out.println(repo.findByName("Virat"));
```

---

### **Derived Query (No JPQL needed)**

```java
List<Student> findByAgeGreaterThan(int age);
System.out.println(repo.findByAgeGreaterThan(20));
```

✔ Spring creates query automatically
✔ No SQL, no JPQL

---

# ## **6️⃣ Update & Delete**

### **UPDATE**

```java
Student s2 = new Student();
s2.setId(2);
s2.setFirstName("Kohli");
s2.setAge(39);

repo.save(s2);   // UPDATE because ID exists
```

### **DELETE**

```java
repo.delete(s2);
```

✔ delete removes the object from DB

---

# ## **7️⃣ JPA in Job App (Simple Explanation)**

In a Job Application system we can have:

### **Entities:**

* Job
* Applicant
* Company
* Resume

### **Repositories:**

```java
JobRepo extends JpaRepository<Job,Integer>
ApplicantRepo extends JpaRepository<Applicant,Integer>
```

### **Operations**

* Create job
* Apply for job
* Search job
* Delete job
* Update job details

All without SQL because JPA does ORM.

---

# ## **8️⃣ Loading Data & Entity**

### **Entity Loading means:**

* Fetching DB row and converting to Java object

Example:

```java
Student s = repo.findById(1).get();
```

✔ Data automatically comes from DB
✔ Hibernate loads the object

---

# ## **9️⃣ Search By Keyword**

### **JPQL Example**

```java
@Query("select s from Student s where s.firstName like %?1%")
List<Student> searchByName(String keyword);
```

Use:

```java
repo.searchByName("ga");  // matches 'Gajanan'
```

✔ `%keyword%` gives partial match like SQL LIKE

---

# 🌟 **KEY BENEFITS OF SPRING DATA JPA**

* No SQL required
* Auto table creation
* CRUD ready
* Custom searches without code complexity
* Works smoothly with Spring Boot

---

