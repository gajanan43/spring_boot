# Project Spring Boot MVC
1) Spring Boot Setup
2) Creating Porduct Model & Table
3) Fetching All Products from DB
4) ResponseEntity
5) Fetch By Product Id
6) Add Product With Image
7) Fetch Product Image
8) Update & Delete Product
9) Search
10) Order Checkout walk through
11) Digram for the Project
12) Running the Application Before Started
13) Creating the DTO's for Oder
14) Creating Model for Order
15) Creating Order Controller
16) Place Order in Service Part 1
17) Place Order in Service Part 2
18) Get All Orders












---
---

# ## **1️⃣ Spring Boot Setup**

Create a new Spring Boot project with:

* Spring Web
* Spring Data JPA
* MySQL Driver
* Thymeleaf (if UI)
* Lombok (optional)

Configure `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/shopdb
spring.datasource.username=root
spring.datasource.password=mysql
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

# ## **2️⃣ Product Model & Table**

### **Product Entity**

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String description;
    private double price;
    private String imagePath; // store uploaded image file path
}
```

### **Repository**

```java
public interface ProductRepo extends JpaRepository<Product, Integer> {}
```

Hibernate automatically creates the product table.

---

# ## **3️⃣ Fetching All Products from DB (Controller)**

```java
@GetMapping("/products")
public ResponseEntity<List<Product>> getAll() {
    return ResponseEntity.ok(productRepo.findAll());
}
```

✔ Returns list of products
✔ No SQL needed

---

# ## **4️⃣ Understanding ResponseEntity**

**ResponseEntity = HTTP response wrapper**

You can return:

* status code
* headers
* body

Example:

```java
return ResponseEntity.status(HttpStatus.OK).body(productList);
```

---

# ## **5️⃣ Fetch Product By Id**

```java
@GetMapping("/products/{id}")
public ResponseEntity<Product> getProduct(@PathVariable int id) {
    return ResponseEntity.of(productRepo.findById(id));
}
```

If ID exists → return product
Else → automatically return 404

---

# ## **6️⃣ Add Product With Image**

### **Upload Controller**

```java
@PostMapping("/products")
public ResponseEntity<String> saveProduct(
        @RequestParam("name") String name,
        @RequestParam("price") double price,
        @RequestParam("file") MultipartFile file) {

    String uploadPath = "images/" + file.getOriginalFilename();
    file.transferTo(new File(uploadPath));

    Product p = new Product();
    p.setName(name);
    p.setPrice(price);
    p.setImagePath(uploadPath);

    productRepo.save(p);
    return ResponseEntity.ok("Product saved");
}
```

✔ Store only **image path** in DB
✔ Actual image stored in local folder

---

# ## **7️⃣ Fetch Product Image**

```java
@GetMapping("/images/{fileName}")
public ResponseEntity<Resource> getImage(@PathVariable String fileName) {
    Resource r = new FileSystemResource("images/" + fileName);
    return ResponseEntity.ok(r);
}
```

✔ Browser can display image
✔ No base64, simple and clean

---

# ## **8️⃣ Update Product**

```java
@PutMapping("/products/{id}")
public ResponseEntity<Product> update(@PathVariable int id, @RequestBody Product p) {
    p.setId(id);
    return ResponseEntity.ok(productRepo.save(p));
}
```

✔ If ID exists → UPDATE
✔ If ID not exists → INSERT

---

# ## **9️⃣ Delete Product**

```java
@DeleteMapping("/products/{id}")
public ResponseEntity<Void> delete(@PathVariable int id) {
    productRepo.deleteById(id);
    return ResponseEntity.noContent().build();
}
```

✔ Deletes record from DB
✔ Returns 204 (no content)

---

# ## **🔍 Search Products**

### Derived Query:

```java
List<Product> findByNameContaining(String keyword);
```

### Use:

```java
@GetMapping("/products/search")
public ResponseEntity<List<Product>> search(@RequestParam String keyword) {
    return ResponseEntity.ok(productRepo.findByNameContaining(keyword));
}
```

✔ Partial search
✔ No manual JPQL

---

# ## **🛍 Order Checkout (Walkthrough)**

Checkout process:

1. User selects products
2. Sends request with product list + qty + user info
3. System calculates:

   * Total price
   * Tax
   * Address
4. Creates **Order record**
5. Saves it in DB

---

# ## **📊 Project Diagram (Simple)**

```
Client (Postman/Website UI)
        │
        ▼
  Product Controller
        │
        ▼
  Product Service
        │
        ▼
  Product Repo (JPA)
        │
        ▼
       MySQL
```

For order:

```
Client → Order Controller → Order Service → Order Repo → DB
```

---

# ## **🟢 Before Running Application**

Make sure:

* DB exists: `shopdb`
* application.properties is correct
* tables are auto-created
* `/products` endpoint returns empty list (first test)

---

# ## **📦 Creating DTOs for Order**

Order DTO = JSON data structure from frontend

```java
public class OrderDTO {
    private List<Integer> productIds;
    private List<Integer> quantities;
    private String address;
}
```

✔ DTO = request body
✔ Not mapped to DB

---

# ## **🧱 Creating Order Model (Entity)**

```java
@Entity
public class OrderDetail {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private LocalDate date;
    private double totalAmount;
    private String address;

    @OneToMany
    private List<Product> products;
}
```

### Order Repository

```java
public interface OrderRepo extends JpaRepository<OrderDetail, Integer> {}
```

---

# ## **🔧 Creating Order Controller**

```java
@PostMapping("/orders")
public ResponseEntity<String> placeOrder(@RequestBody OrderDTO dto) {
    orderService.placeOrder(dto);
    return ResponseEntity.ok("Order Placed");
}
```

---

# ## **🧠 Place Order — Service (Part 1)**

```java
@Service
public class OrderService {
    @Autowired ProductRepo productRepo;
    @Autowired OrderRepo orderRepo;

    public void placeOrder(OrderDTO dto) {

        List<Product> products = productRepo.findAllById(dto.getProductIds());

        double total = products.stream()
                .mapToDouble(Product::getPrice)
                .sum();
```

Calculates price.

---

# ## **🧠 Place Order — Service (Part 2)**

```java
        OrderDetail order = new OrderDetail();
        order.setProducts(products);
        order.setTotalAmount(total);
        order.setAddress(dto.getAddress());
        order.setDate(LocalDate.now());

        orderRepo.save(order);
    }
}
```

✔ Saves full order
✔ No manual SQL

---

# ## **📋 Get All Orders**

```java
@GetMapping("/orders")
public ResponseEntity<List<OrderDetail>> getOrders() {
    return ResponseEntity.ok(orderRepo.findAll());
}
```

---

# 🌟 **YOU NOW HAVE A COMPLETE MINI E-COMMERCE PROJECT**

### You have:

✔ Product CRUD
✔ Image upload
✔ Search
✔ Order placement
✔ DTO
✔ Controllers
✔ Services
✔ Entities
✔ Diagram
✔ ResponseEntity usage
✔ Spring Boot + MVC + JPA

---

