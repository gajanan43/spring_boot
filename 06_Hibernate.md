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
---
---

# Interview Topics:
1) Mapping
2) Fetch Types
3) Hibernate States (Transient, Persistent, Detached)
4) Caching (L1, L2)
5) Cascade Types

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
---


# 🔥 **Hibernate Object States**

Hibernate uses 3 states to describe the lifecycle of an object:

---

# ✅ **1. Transient State**

Object is **not associated with Hibernate Session**, and **not saved** in the database.

### ✔ Characteristics

* No primary key assigned (ID = null)
* Not in Session
* Not stored in database

### ✔ Example

```java
Student s = new Student();   // new keyword
s.setName("Ram");            // Transient
```

At this moment:
✔ Object is in Java memory
✘ Not saved in DB
✘ Session doesn’t know about it

---

# ✅ **2. Persistent State**

Object is **associated with a Hibernate Session**, and Hibernate tracks it.

### When it becomes Persistent?

✔ After calling `session.save()`
✔ After calling `session.persist()`
✔ When loading from DB (`session.get()` or `session.load()`)

### ✔ Example

```java
Session session = factory.openSession();
session.beginTransaction();

Student s = new Student();       // Transient
s.setName("Ram");

session.save(s);                 // Now Persistent
```

Now the object is:
✔ Linked with Session
✔ Stored in DB
✔ Any changes to object are automatically updated

### ✔ Example (auto-update)

```java
s.setName("Raj");   // Hibernate automatically updates in DB when transaction commits
```

---

# ❌ **3. Detached State**

Object was **Persistent earlier**, but **Session is closed** or object is removed from Session.

### When it becomes Detached?

✔ After calling `session.close()`
✔ After calling `session.clear()`
✔ After calling `session.evict(object)`

### ✔ Example

```java
session.close();     // Now Student object is Detached
s.setName("Rakesh"); // No automatic update
```

Now changes **do not** reflect in DB unless you reattach it.

---

# 🔁 **Reattaching Detached Object**

Use:

* `session.update(object)`
* `session.merge(object)`

Example:

```java
Session session2 = factory.openSession();
session2.beginTransaction();

session2.update(s); // Now it becomes Persistent again
```

---

# 🧠 Quick Comparison Table

| State          | In DB? | In Session? | Auto Update? |
| -------------- | ------ | ----------- | ------------ |
| **Transient**  | ❌ No   | ❌ No        | ❌ No         |
| **Persistent** | ✔ Yes  | ✔ Yes       | ✔ Yes        |
| **Detached**   | ✔ Yes  | ❌ No        | ❌ No         |

---

# 🎯 Most Important Interview Questions

### **1. What are the Hibernate states?**

Transient, Persistent, Detached.

### **2. Difference between Transient and Detached?**

* Transient → never stored in DB
* Detached → stored earlier but Session closed

### **3. How to convert Detached to Persistent?**

`session.update()` or `session.merge()`.


---
---


# 🔥 **Hibernate Caching (Very Important Topic)**

Hibernate uses cache to **reduce database calls** and improve performance.

There are two main caches:

---

# ✅ **1. Level 1 Cache (L1 Cache)**

### ✔ **Default cache**

* Enabled by default
* Cannot be disabled
* Works **per Session**
* Each session has its **own** L1 cache

### ✔ What is cached?

Objects that you load using:

```java
session.get(Student.class, 1);
```

### ✔ Example

```java
Session session = factory.openSession();

Student s1 = session.get(Student.class, 1);  // DB hit
Student s2 = session.get(Student.class, 1);  // No DB hit (from L1 cache)
```

### ✔ L1 Cache clears when:

* `session.clear()`
* `session.evict(object)`
* `session.close()`

---

# 🔥 **Most Important Interview Point**

**Level 1 cache works ONLY with one session.**
If you open a new session → cache is empty.

---

# 🟦 **2. Level 2 Cache (L2 Cache)**

### ✔ Not enabled by default

### ✔ Shared across sessions

### ✔ Slower than L1 but reduces DB calls among multiple sessions

To enable L2 cache, you must add a caching provider like:

* EhCache
* Infinispan
* OSCache
* Redis (new setups)

---

# ✔ **Example: Enabling Level 2 Cache**

### **Step 1: Add dependency**

(EhCache Example)

```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-ehcache</artifactId>
    <version>6.x.x</version>
</dependency>
```

---

### **Step 2: Enable second-level cache in config**

```xml
<property name="hibernate.cache.use_second_level_cache">true</property>
<property name="hibernate.cache.region.factory_class">
    org.hibernate.cache.ehcache.EhCacheRegionFactory
</property>
```

---

### **Step 3: Mark an entity as cacheable**

```java
@Entity
@Cacheable
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Student {
    @Id
    private int id;

    private String name;
}
```

Now, this entity supports L2 caching.

---

# ✔ Example Scenario (L2 Cache Working)

### Session 1:

```java
Session s1 = factory.openSession();
Student st1 = s1.get(Student.class, 1);   // DB hit
s1.close();
```

### Session 2:

```java
Session s2 = factory.openSession();
Student st2 = s2.get(Student.class, 1);   // No DB hit (from L2 cache)
```

L2 Cache is **shared across sessions**, so it prevents repeated DB hits.

---

# 📌 **L1 vs L2 Cache Comparison**

| Feature       | L1 Cache        | L2 Cache                |
| ------------- | --------------- | ----------------------- |
| Default       | ✔ Yes           | ❌ No                    |
| Per Session   | ✔ Yes           | ❌ No (shared)           |
| When Cleared? | session.close() | app shutdown / manual   |
| Speed         | Fastest         | Fast                    |
| Need Config?  | No              | Yes                     |
| Cache scope   | Session         | SessionFactory (global) |

---

# 🎯 Top Interview Questions (Caching)

1. **Is Level 1 cache mandatory?**
   ✔ Yes, always enabled.

2. **Is Level 2 cache mandatory?**
   ❌ No, must be configured manually.

3. **Can L1 and L2 work together?**
   ✔ Yes, L1 is checked first, then L2.

4. **Does `session.clear()` clear L2 cache?**
   ❌ No, only L1 cache is cleared.

5. **What happens on session.close()?**
   ✔ L1 cache is destroyed
   ❌ L2 cache remains alive

---
---


# 🔥 **Cascade Types in Hibernate**

Cascade means:
👉 When you perform an operation on **Parent**, Hibernate automatically performs the same operation on **Child**.

Example:
Delete a **Student**, automatically delete **Laptop**.

---

# ✅ **List of Cascade Types**

| Cascade Type | Meaning                             |
| ------------ | ----------------------------------- |
| `PERSIST`    | Save parent → child is also saved   |
| `MERGE`      | Merge parent → child is also merged |
| `REMOVE`     | Delete parent → child is deleted    |
| `REFRESH`    | Refresh parent → refresh child      |
| `DETACH`     | Detach parent → child detached      |
| `ALL`        | Applies all operations              |

---

# 📌 Example Entity (Parent → Child)

### **Student (Parent)**

```java
@OneToMany(mappedBy = "student", cascade = CascadeType.ALL)
private List<Laptop> laptops;
```

### **Laptop (Child)**

```java
@ManyToOne
@JoinColumn(name = "student_id")
private Student student;
```

---

# 🇦 **Understanding Each Cascade Type**

---

# 1️⃣ **CascadeType.PERSIST**

When parent is saved → child is automatically saved.

### Example:

```java
Student s = new Student();
Laptop l = new Laptop();
s.getLaptops().add(l);   // add child
l.setStudent(s);

session.persist(s);      // only saving parent
```

✔ Laptop is also saved automatically.

---

# 2️⃣ **CascadeType.MERGE**

When parent is merged → child is merged.

Use case: when both parent & child were detached, and you reattach the parent.

```java
session.merge(student);
```

✔ Updates happen on both parent and children.

---

# 3️⃣ **CascadeType.REMOVE**

Delete parent → delete child.

```java
session.remove(student);
```

✔ Child (Laptop) will also be deleted.

⚠ Without cascade = REMOVE
Hibernate throws **ConstraintViolationException**.

---

# 4️⃣ **CascadeType.REFRESH**

Reload fresh data from database for parent and child.

```java
session.refresh(student);
```

---

# 5️⃣ **CascadeType.DETACH**

Detach parent → detach all children.

```java
session.detach(student);
```

Child becomes **detached** too.

---

# 6️⃣ **CascadeType.ALL**

Shortcut for **PERSIST + MERGE + REMOVE + REFRESH + DETACH**

Most commonly used.

```java
cascade = CascadeType.ALL
```

---

# 🧠 **Most Important Interview Questions**

### **1. What is CascadeType.ALL?**

A shortcut for all cascades (persist, merge, remove, detach, refresh).

### **2. What is the most dangerous cascade?**

`CascadeType.REMOVE`
Because deleting parent deletes all children.

### **3. Does cascade affect fetching?**

❌ No
Cascade is for operations
FetchType is for loading.

### **4. When do we use cascade?**

When child lifecycle is fully dependent on parent.

Example:
Student → Laptop (dependent)
Order → OrderItems

---







