# ✅ **Spring Boot Web With JSP 

## **1) Create a Spring Boot Project**

* Select dependencies: **Spring Web**
* JSP needs an **embedded Tomcat + Jasper** to run JSP pages.

---

## **2) Create a JSP page**

Spring Boot does **not** load JSP from `static/` or `templates/`.

Correct folder structure:

```
src/main/
   └── webapp/
        └── index.jsp
```

**index.jsp**

```jsp
<%@page language="java" %>
<html>
<body>
<h2>Hello World!!!</h2>
</body>
</html>
```

---

## **3) Create a Controller**

❌ Wrong:

```java
return "index.jsp";
```

✔ Correct:

* You should return only the **view name** (no `.jsp`)
* Use `@RequestMapping`

```java
@Controller
public class HomeController {

    @RequestMapping("/")
    public String home() {
        System.out.println("home method called");
        return "index";  // view name
    }
}
```

---

## **Why JSP was downloading?**

Because JSP engine was not present.

✔ Add **Tomcat Jasper** dependency:

```xml
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```

✔ JSP engine version must match the **embedded Tomcat version** in Spring Boot.

---

## **4) Sending Data From JSP to Controller**

Use HTML form:

```jsp
<form action="add">
    <input type="text" name="num1">
    <input type="text" name="num2">
    <button type="submit">Add</button>
</form>
```

---

## **5) Accepting Data (Servlet Style)**

Using `HttpServletRequest`:

```java
@RequestMapping("/add")
public String add(HttpServletRequest req) {
    int n1 = Integer.parseInt(req.getParameter("num1"));
    int n2 = Integer.parseInt(req.getParameter("num2"));
    int result = n1 + n2;

    HttpSession session = req.getSession();
    session.setAttribute("result", result);

    return "result";
}
```

---

## **6) Display Data On Result Page**

Using JSP:

```jsp
Result is : <%= session.getAttribute("result") %>
```

or with EL:

```jsp
Result is : ${result}
```

---

## **7) @RequestParam (Better Method)**

No need for `request.getParameter()`:

```java
@RequestMapping("/add")
public String add(@RequestParam("num1") int num1,
                  @RequestParam("num2") int num2,
                  HttpSession session) {

    session.setAttribute("result", num1 + num2);
    return "result";
}
```

---

## **8) Model Object**

Better than using session.

```java
@RequestMapping("/add")
public String add(@RequestParam int num1,
                  @RequestParam int num2,
                  Model model) {

    model.addAttribute("result", num1 + num2);
    return "result";
}
```

✔ No session needed
✔ Works only for one request → cleaner

---

## **9) Setting Prefix & Suffix**

In **application.properties**:

```properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

Folder:

```
src/main/webapp/WEB-INF/views/index.jsp
```

Controller:

```java
return "index";   // not index.jsp
```

---

## **10) ModelAndView**

Another way to pass data + view in same object.

```java
@RequestMapping("/add")
public ModelAndView add(@RequestParam int num1,
                        @RequestParam int num2) {

    ModelAndView mv = new ModelAndView();
    mv.setViewName("result");
    mv.addObject("result", num1 + num2);
    return mv;
}
```

---

## **11) Need for @ModelAttribute**

Used when:

* You have a **form with many fields**
* Want to bind form data to a Java object

Example:

```java
public class User {
    private String name;
    private int age;
}
```

---

## **12) Using @ModelAttribute**

Controller:

```java
@RequestMapping("/addUser")
public String addUser(@ModelAttribute User user, Model model) {
    model.addAttribute("user", user);
    return "result";
}
```

JSP:

```jsp
<p>Name: ${user.name}</p>
<p>Age: ${user.age}</p>
```

---

## **13) Spring Boot With Thymeleaf**

Thymeleaf is the recommended alternative to JSP.

Advantages:

* Works inside `templates/`
* No Jasper dependency
* Much cleaner syntax
* Automatically supported by Spring Boot

Example:

```html
<p th:text="${result}"></p>
```

---

# ✅ Final Clean Summary

| Topic            | Status                                                      |
| ---------------- | ----------------------------------------------------------- |
| JSP folder       | `src/main/webapp/`                                          |
| Need Jasper?     | Yes                                                         |
| Return view name | `"index"` not `"index.jsp"`                                 |
| Data transfer    | `Model`, `ModelAndView`, `@RequestParam`, `@ModelAttribute` |
| Session          | Only if needed                                              |
| Prefix & Suffix  | Makes view return easier                                    |
| Thymeleaf        | Better than JSP                                             |

---

