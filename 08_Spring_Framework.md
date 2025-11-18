# Spring Framework:

- Create a spring.xml file and write beans(for object creation with their class)


## Topic: 
### 1) Scope(Singleton & prototype):

**Bean Scope** defines **how many objects** of a particular bean Spring should create and **how long** that object should live in the application.

You basically tell Spring:
👉 *"How many times should this bean object be created?"*

🚀 **1. Singleton (Default Scope)**

* **Only one object** is created for the entire Spring container.
* Same object is returned every time you request that bean.

### Example:

```xml
<bean id="alien" class="org.example.Alien" scope="singleton"/>
```

### When to use?

✔ Services
✔ Repositories
✔ Utility classes

---

🚀 **2. Prototype**

* A **new object** is created **every time** the bean is requested.

### Example:

```xml
<bean id="alien" class="org.example.Alien" scope="prototype"/>
```

### When to use?

✔ When you need fresh objects
✔ For stateful beans

---

### 2) Setter Injection(Property):

```
<property name="age" value="22"> </property>
 ```
- Set private value using this.

### 3) Ref Attribute:

```
<property name="lap" ref="lap1"> </property>
```
- It is used to injecting the object
- For this two beans must created

### 4) Constructor Injection:

```

```

