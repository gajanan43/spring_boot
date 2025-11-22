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
- Tomcat Jasper is depends on which tomcat server running(Tomacat version=Tocat Jasper Version).

## 4) Sending data to controller:
- I created form it accept two values from user.
  
## 5) Accepting Data the servlet way
- Accepting data from user through HttpServletRequest object 

## 6) Display Data on Result Page
- To mantain values between pages needs to session
- By using HttpSession object maintains
- in jsp page accpet like ```<%= session.getAttribute("result")%>``` OR ${result}. 

## 7) RequestParam
- 

## 8) Model Object

## 9) Setting Prefix & Suffix

## 10) ModelAndView

## 11) Need for ModelAttribute

## 12) Using ModelAttribute

## 13) Spring Boot With Thymeleaf

