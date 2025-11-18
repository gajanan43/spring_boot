# Java Based configuration:

## 1) create package config in that create a AppConfig(config.AppConfig)

```
AppConfig.java

@Configuration
public class AppConfig {
    @Bean
    public Desktop desktop() {
        return new Desktop();
    }
}

Main.java

pplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
Desktop desktop = context.getBean(Desktop.class);
desktop.code();

```

## 2) BeanName:

```
OPTIONS:

1) Name of the bean will the same of method name

Desktop desktop = context.getBean("desktop",Desktop.class);

AppConfig.java

@Configuration
public class AppConfig {
    @Bean
    public Desktop desktop() {
        return new Desktop();
    }
}

2) Name of bean same at a Bean Notation

Desktop desktop = context.getBean("desktop1",Desktop.class);

@Configuration
public class AppConfig {
    @Bean("desktop1")
    public Desktop desktop() {
        return new Desktop();
    }
}

3) we can set array of beans with a multiple names

Desktop desktop = context.getBean("man",Desktop.class);

@Bean({"ram","sham","gun","man"})

```
## 3) Scope Annotation(Two object run & output also two)

```
Desktop desktop = context.getBean(Desktop.class);
desktop.code();

Desktop desktop1 = context.getBean(Desktop.class);
desktop1.code();

@Configuration
public class AppConfig {
    @Bean
    @Scope("prototype")
    public Desktop desktop() {
        return new Desktop();
    }
}
```
## 4) Autowire(Connection between computer interface(work Desktop class beacuse it has created a Bean) with Alien class)

```
Alien obj1= context.getBean(Alien.class);
  System.out.println(obj1.getAge());
  obj1.complier();


@Configuration
public class AppConfig {
@Bean
public Alien Alien(@Autowired Computer comp) {
  Alien obj1= new Alien();
  obj1.setAge(22);
  obj1.setComp(comp);
  return obj1;
}

@Bean
public Desktop desktop() {
  return new Desktop();
  }
}

```

## 5) Primary & Qulifier:

```
@Configuration
public class AppConfig {
@Bean
public Alien Alien(@Autowired Computer comp) {   //@Qualifier("desktop") -> It is on @Autowired -> It means desktop beans run(object uses a desktop)
  Alien obj1= new Alien();
  obj1.setAge(22);
  obj1.setComp(comp);
  return obj1;
}

@Bean
public Desktop desktop() {
  return new Desktop();
}

@Bean
@Primary   ---> This means laptop bean run(object uses a laptop)
public Laptop  laptop() {
  return new Laptop();
  }
}

```

## 6) Component of Sterotype Annotation(No need to create Beans)

```
@Component ----> On all The classes

@Configuration
@ComponentScan("org.example")
public class AppConfig {
    // No Body
}

```

## 7) Autowired Filed, Constructor, Setter:(Most preferable Atuowird on setter)

```
Solution 1:  Using quilifier run laptop bean(object) || Otherwise @Component delete from Desktop
@Autowired
@Qualifier("laptop")   ---> name of class with Small Case
private Computer comp;

```

## 8) Primary Annotation:(If write a Quilifer & Primary ++++++++++ Quilifer get a first Priority )

```
@Primary  ----> On the top of Class which you want to run
```

## 9) Scope & Value Annotation:

```
@Scope("prototype") ----> It is used two object of same class run with differently(allow to run bean run two times)
```

```
@Value("25")      ---> To assign the value of the private members
private int age;
```
