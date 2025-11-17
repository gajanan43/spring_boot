# Hibernate:

- Hibernate is ORM(Object Relation Mapping) Framework.
- It is solve the problem of jdbc beacuse there in write lot of qurey.

## Topic:
1) Hibernate connect to DB
2) Perform CRUD operation
3) Changing the table & cloumn name
4) Embeddable
5) Mapping(OneToOne, OneToMany, ManyToOne, ManyToMany)
6) Eager & Lazy fetch
7) Hibernate Caching(L1 cache(Enabled by default (Session-level)) & L2 caching)
8) HQL(Hibernate Query Language)
9) Get VS Load
10) L2 cache using Ehcache   


## Setup: 

1) Create a project into maven
2) Add MYSQL dependencies into pom.xml file
3) Create a class file for DB(Write a structure of DB)
4) Create a hibernate.cfg.xml file inside resources folder(write a connection property for DB)
5) Write a main file(CURD operation)


## Method in Hibernate:
1) persist -> To insert data into the DB
2) merge  -> Update the data in DB(if data is new then this work as insert query)
3) remove  -> Delete the data from DB
4) get  -> View data from DB
5) find  -> Find the specific data from DB

```
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;


public class Main {
    public static void main(String [] args) {

        Student s1=new Student();
//        s1.setId(4);
//        s1.setName("You");
//        s1.setEmail("demo4@gmail.com");

//        Student s2=null; // Create a null object to read a specific row

//        Configuration cfg=new Configuration();
//        cfg.addAnnotatedClass(org.example.Student.class);
//        cfg.configure();

        SessionFactory sf= new  Configuration()
                .addAnnotatedClass(org.example.Student.class)
                .configure()
                .buildSessionFactory();   // cfg.buildSessionFactory();

        Session session=sf.openSession();
//      s2=session.get((Student.class,4);  //Read Query
        s1=session.find(Student.class,4); //find object to the delete data realted object
        Transaction tx= session.beginTransaction();

//        session.merge(s1); // Update & insert Query
        session.remove(s1); // Delete query
//        session.persist(s1);    // Insert Query
        tx.commit();
        sf.close();
        session.close();
        System.out.println(s1);
//      System.out.println(s2);
    }
}

```


## Notations in Hibernate:
```
1) Entity(name=" ")  ->    Change the table name
2) Id                ->    Delcare the primary key in the table
3) Cloumn(name=" ")  ->    Change the column name
4) Transit           ->    Skip this column in the table
5) Table(name=" ")   ->    Change the table name
6) Embeddable        ->    Merge two seperate tables into single(This notation write which one connect)

```


# Mapping:

# ✅ **1) One-To-One**

### ✔ Meaning: One person → One passport

### **Person.java**

```java
@OneToOne
@JoinColumn(name = "passport_id")
private Passport passport;
```

### **Passport.java**

```java
@OneToOne(mappedBy = "passport")
private Person person;
```

---

# ✅ **2) One-To-Many**

### ✔ Meaning: One student → Many laptops

### **Student.java**

```java
@OneToMany(mappedBy = "student")
private List<Laptop> laptops;
```

### **Laptop.java**

```java
@ManyToOne
@JoinColumn(name = "student_id")
private Student student;
```

---

# ✅ **3) Many-To-One**

### ✔ Meaning: Many laptops → One student

(Reverse of One-to-Many)

### **Laptop.java**

```java
@ManyToOne
@JoinColumn(name = "student_id")
private Student student;
```

---

# ✅ **4) Many-To-Many**

### ✔ Meaning: Students ↔ Courses (both many)

### **Student.java**

```java
@ManyToMany
@JoinTable(
    name = "student_course",
    joinColumns = @JoinColumn(name = "student_id"),
    inverseJoinColumns = @JoinColumn(name = "course_id")
)
private List<Course> courses;
```

### **Course.java**

```java
@ManyToMany(mappedBy = "courses")
private List<Student> students;
```

---

# 🎯 **Super Simple Summary**

| Mapping          | Meaning     | Real-Life Example  |
| ---------------- | ----------- | ------------------ |
| **One-To-One**   | 1 → 1       | Person → Passport  |
| **One-To-Many**  | 1 → Many    | Student → Laptops  |
| **Many-To-One**  | Many → 1    | Laptops → Student  |
| **Many-To-Many** | Many ↔ Many | Students ↔ Courses |

---

# ✅ **Fetch Types in Hibernate**

Hibernate has **two fetch types**:

---

# **1. LAZY Fetch (Default for Collections)**

👉 Data is loaded **only when required** (on demand).
👉 Hibernate creates a **proxy** object.
👉 Saves memory & improves performance.

### **Example**

```java
@Entity
public class Student {

    @OneToMany(mappedBy = "student", fetch = FetchType.LAZY)
    private List<Laptop> laptops;
}
```

### When you call:

```java
student.getLaptops();
```

Only then Hibernate hits the database to load `laptops`.

---

# **2. EAGER Fetch (Immediate Loading)**

👉 Loads related (child) entity **immediately** with parent.
👉 Slower because it loads everything at once.
👉 Should be avoided for large collections.

### Example

```java
@Entity
public class Student {

    @OneToMany(mappedBy = "student", fetch = FetchType.EAGER)
    private List<Laptop> laptops;
}
```

Now when you load student:

```java
Student s = session.get(Student.class, 1);
```

Hibernate **immediately fetches laptops** too (even if you don’t use them).

---

# 📌 **Default Fetch Types**

| Mapping Type  | Default Fetch Type |
| ------------- | ------------------ |
| `@OneToOne`   | EAGER              |
| `@ManyToOne`  | EAGER              |
| `@OneToMany`  | LAZY               |
| `@ManyToMany` | LAZY               |

---

# ✔ Interview Question

**Q: Which fetch type is better?**
**A: LAZY**, because it loads only required data and avoids heavy joins.

---

# ✔ Best Practice

Always use LAZY fetch type unless you specifically need EAGER.

---






