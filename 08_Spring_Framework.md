# Spring Framework:

- Create a spring.xml file and write beans(for object creation with their class)
- create this file inside the resource folder


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

### 6) AutoWiring 

```
1) byName:  it serach on the basis which will match with obj & if you wirte explict property then work explict property

<bean id="alien" class="org.example.Alien" autowire="byName">
<bean id="comp1" class="org.example.Laptop"> </bean>
<bean id="comp" class="org.example.Desktop"> </bean>

2) byType: It Works on which type of class(match with laptop class run this)

<bean id="alien" class="org.example.Alien" autowire="byType">
<bean id="comp1" class="org.example.Laptop"> </bean>

```
- If two or one beans with different name of object then it cann't work it(comp1 & comp2)
- If two beans create autowiring using byType then shows error

### 7) Primary Beans:

```
<bean id="alien" class="org.example.Alien" autowire="byType">
<bean id="comp1" class="org.example.Laptop" primary="true"> </bean>
<bean id="comp" class="org.example.Desktop"> </bean>

```
- Whenever you have a confusion between them uses primar beans

### 8) Lazy int Beans:

```
<bean id="comp2" class="org.example.Desktop" lazy-init="true"> </bean>
```
- It is called util the explict object create.
- It call the when in the property called.

### 9) Getbean ByType

```
1) Alien obj1= (Alien) context.getBean("alien");
2) Alien obj1= context.getBean(Alien.class);
3) Alien obj1= context.getBean("alien",Alien.class);

```
- First one We had to typecast the returned object because getBean() returns Object.
- Secon one by using this avoid typecasting & if want to work beans 
- Third one This still avoids typecasting and ensures type safety.
  
### 10) Inner Beans:

```
<bean id="alien" class="org.example.Alien" autowire="byType">
    <property name="comp">
           <bean id="comp1" class="org.example.Laptop" primary="true"> </bean>
    </property>
</bean>

```
- comp1 bean only work for Alien class
