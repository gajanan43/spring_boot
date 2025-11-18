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
Solutions:

1) Int this solution if i change the order passed value of constructor then shows error

<constructor-arg value="21"/>
<constructor-arg ref="lap1"/>

2) If constructor take two int parameter then again problem

<constructor-arg type="org.example.Laptop" ref="lap1"/>
<constructor-arg type="int" value="21"/>

3) Best solution using index

<constructor-arg index="1" ref="lap1"/>
<constructor-arg index="0" value="21"/>

4) Work on sequence of parameter if change the sequence then

<constructor-arg name="lap" ref="lap1"/>
<constructor-arg name="age" value="21"/>

@ConstructorProperties({"age","lap"})
 public Alien(int age, Laptop lap) {
    this.age = age;
    this.lap = lap;
}

```

- It is used to passes the value to constructor

### 5) Creating Interface:

### 6)
