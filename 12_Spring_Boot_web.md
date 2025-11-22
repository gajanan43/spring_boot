# Spring Boot Web:

## 1) Create a Spring Boot app project(Spring Web dependencies)
## 2) Create a JSP page

- create forlder webapp on main foder, in webapp create index.jsp file
```
index.jsp

<%@page language="java" %>
<html>
<body>
<h2>Hello World!!!</h2>
</body>
</html>
```

## 3) Create a Controller:

```
@Controller
public class HomeController {

    public String home(){
        System.out.println("home method called");
        return "index.jsp";
    }
}
```
- It is not working
  
## 3) RequestMapping:

```
@Controller
public class HomeController {
    @RequestMapping("/")
    public String home(){
        System.out.println("home method called");
        return "index.jsp";
    }
}
```
- It is working but download a index.jsp file
- For this run JSP to Servlet by using a ```Tomcat Jasper``` is denpendencies intall into project.

## 4) Sending data to controller:

## 5)
