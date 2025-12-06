# Spring Data REST:

1) Creating a Data REST Project
2) Running The Project
3) Update & Delete

- No Need a ```Service,Controller in Project Setup```.

---
---

# ## **1️⃣ What is Spring Data REST?**

Spring Data REST automatically exposes your **JPA Repositories as REST APIs**.

> ⚡ **No Controller, No Service, No ResponseEntity — still you get full CRUD REST API.**

---

# ## **2️⃣ Creating a Spring Data REST Project**

### **Dependencies required**

✔ Spring Web
✔ Spring Data JPA
✔ MySQL Driver
✔ Spring Data REST (important)

---

# ## **3️⃣ application.properties**

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/shopdb
spring.datasource.username=root
spring.datasource.password=mysql
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

# ## **4️⃣ Create Entity (like normal JPA)**

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String name;
    private double price;
}
```

---

# ## **5️⃣ Create Repository (Very Important)**

```java
@RepositoryRestResource
public interface ProductRepo extends JpaRepository<Product, Integer> {}
```

👉 `@RepositoryRestResource` tells Spring:

> **Expose repository as REST API automatically**

---

# ## **6️⃣ Run The Project**

Start the app and open:

```
http://localhost:8080
```

You will see:

```
/products
```

This is auto-created REST endpoint for your entity.

---

# ## **7️⃣ GET All Products**

```
GET  http://localhost:8080/products
```

Spring returns JSON list automatically
✔ No Controller
✔ No Service
✔ No ResponseEntity

---

# ## **8️⃣ GET Product By ID**

```
GET  http://localhost:8080/products/1
```

---

# ## **9️⃣ ADD (POST) Product**

```
POST  http://localhost:8080/products
Content-Type: application/json

{
  "name": "Laptop",
  "price": 45000
}
```

✔ No controller logic needed
✔ It automatically inserts into DB

---

# ## **🔁 UPDATE Product (PUT)**

```
PUT  http://localhost:8080/products/1
Content-Type: application/json

{
  "name": "Laptop Pro",
  "price": 56000
}
```

✔ Spring updates record automatically

---

# ## **❌ DELETE Product**

```
DELETE  http://localhost:8080/products/1
```

✔ Deletes product without controller code

---

# ## 🌟 **KEY BENEFITS**

✔ Auto CRUD REST endpoints
✔ No **Controller** class
✔ No **Service** class
✔ No **ResponseEntity** code
✔ Less boilerplate
✔ Very fast prototyping
✔ Perfect for **small applications, testing, demos**

---

# ## ⚠️ When to Use Spring Data REST?

👍 Best for:

* Quick prototypes
* Admin tools
* Simple microservices
* Postman testing

👎 Not recommended for:

* Complex business logic
* Authentication
* Custom validations
* Payment flow
* Complex transactional operations

➡ In those cases you need **normal Controller + Service**

---

# ⭐ FINAL UNDERSTANDING

> **Spring Data REST lets your Repository act like a full REST API**, so you don’t write Service, Controller, ResponseEntity, or custom SQL.

Just:

* Define **Entity**
* Define **Repository**
* Spring exposes all REST endpoints automatically

---

