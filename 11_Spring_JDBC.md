# Spring JDBC:

## 1) Creating a Spring JDBC Project(JDBC API, MYSQL Driver & Create a Student Class)
## 2) Student Service & Repo
## 3) JDBC Template
## 4) Schema & Data files
## 5) Rowmapper

---
---

## 1️⃣ Creating a Spring JDBC Project

*(JDBC API, MySQL Driver & Student Class)*

### What this means (simple):

You create a Spring project that can **talk to a MySQL database using JDBC**.

### What you add:

* **JDBC API** → to run SQL queries
* **MySQL Driver** → to connect to MySQL
* **Student class** → to hold student data

### Example Student class:

```java
public class Student {
    private int id;
    private String name;
    private int age;
}
```

👉 This class represents **one row in the student table**.

---

## 2️⃣ Student Service & Repository

### Repository (DAO)

* Contains **SQL logic**
* Talks directly to database

```java
@Repository
public class StudentRepo {
    // DB code here
}
```

### Service

* Contains **business logic**
* Calls repository methods

```java
@Service
public class StudentService {
    // calls StudentRepo
}
```

👉 Controller → Service → Repository → Database

---

## 3️⃣ JDBC Template

### What is JdbcTemplate?

`JdbcTemplate` is a **Spring helper class** that:

* Removes boilerplate JDBC code
* Handles connection, statement, result set automatically

### Example:

```java
@Autowired
JdbcTemplate jdbcTemplate;
```

### Insert example:

```java
jdbcTemplate.update(
    "INSERT INTO student VALUES (?,?,?)",
    student.getId(),
    student.getName(),
    student.getAge()
);
```

👉 You write SQL, Spring handles everything else.

---

## 4️⃣ Schema & Data Files

### schema.sql

* Used to **create table**

```sql
CREATE TABLE student (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

### data.sql

* Used to **insert initial data**

```sql
INSERT INTO student VALUES (1, 'Gajanan', 22);
INSERT INTO student VALUES (2, 'Omkar', 21);
```

👉 Spring automatically runs these files at startup.

---

## 5️⃣ RowMapper

### What is RowMapper?

* Converts **database row → Java object**

### Why needed?

Database returns rows
Java needs objects

### Example:

```java
RowMapper<Student> rowMapper = (rs, rowNum) -> {
    Student s = new Student();
    s.setId(rs.getInt("id"));
    s.setName(rs.getString("name"));
    s.setAge(rs.getInt("age"));
    return s;
};
```

### Usage:

```java
List<Student> students =
    jdbcTemplate.query("SELECT * FROM student", rowMapper);
```

---

## 🔁 Overall Flow (Very Simple)

```
Controller
   ↓
Service
   ↓
Repository
   ↓
JdbcTemplate
   ↓
MySQL Database
```

---

## 🎯 One-Line Interview Explanation

> *I built a Spring JDBC project using JdbcTemplate to perform database operations, used schema and data files for table creation, and RowMapper to convert database rows into Java objects.*

