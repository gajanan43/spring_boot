# OneToOne

```
@Entity
public class Alien {

    @Id
    private int id;

    private int age;
    private String name;

    @OneToOne
    private Laptop laptops;
}

@Entity
public class Laptop {

    @Id
    private int lid;

    private String brand;
    private int ram;
    private String model;
}

public class Main {
    public static void main(String[] args){
        Laptop l1=new Laptop();
        l1.setLid(101);
        l1.setModel("HP");
        l1.setBrand("12 Generation");
        l1.setRam(18);

        Alien obj=new Alien();
        obj.setId(1);
        obj.setAge(22);
        obj.setName("Gajanan");
        obj.setLaptop(l1);

       
        session.persist(obj);
       
  }
}

```

# OneToMany:

```
@Entity
public class Laptop {

    @Id
    private int lid;

    private String brand;
    private int ram;
    private String model;
}

@Entity
public class Alien {

    @Id
    private int id;

    private int age;
    private String name;

    @OneToMany(cascade = CascadeType.ALL)
    @JoinColumn(name = "alien_id")  // FK in Laptop table
    private List<Laptop> laptops;
}


  Laptop l1 = new Laptop();
  l1.setBrand("HP");
  l1.setModel("12 Gen");
  l1.setRam(18);
  
  Laptop l2 = new Laptop();
  l2.setBrand("Dell");
  l2.setModel("13 Gen");
  l2.setRam(16);
  
  Alien alien = new Alien();
  alien.setName("Gajanan");
  alien.setAge(22);
  alien.setLaptops(Arrays.asList(l1, l2));
  
  Transaction tx = session.beginTransaction();
  session.persist(alien);   // ✅ ONLY THIS
  tx.commit();

```

# ManyToOne:

```
@Entity
public class Alien {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private int age;
    private String name;

    // getters & setters
}

@Entity
public class Laptop {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int lid;

    private String brand;
    private int ram;
    private String model;

    @ManyToOne
    @JoinColumn(name = "alien_id")   // FK column
    private Alien alien;   // ✅ SINGLE Alien

    // getters & setters
}

Alien a1 = new Alien();
a1.setName("Gajanan");
a1.setAge(22);

Alien a2 = new Alien();
a2.setName("Omkar");
a2.setAge(22);

Laptop l1 = new Laptop();
l1.setBrand("Dell");
l1.setModel("11 Generation");
l1.setRam(32);
l1.setAlien(a1);   // ONE alien per laptop

Laptop l2 = new Laptop();
l2.setBrand("HP");
l2.setModel("12 Generation");
l2.setRam(16);
l2.setAlien(a1);   // SAME alien → many laptops

Transaction tx = session.beginTransaction();
session.persist(a1);
session.persist(a2);
session.persist(l1);
session.persist(l2);
tx.commit();

```

# ManyToMany:

```
@Entity
public class Laptop {
    @Id
    private int lid;
    private String brand;
    private int ram;
    private String model;
    @ManyToMany
    private List<Alien> aliens;
}

@Entity
public class Alien {
    @Id
    private int id;
    private int age;
    private String name;
    @ManyToMany(mappedBy = "aliens")
    private List<Laptop> laptops;
}

Alien a1 = new Alien();
        a1.setId(1);
        a1.setName("Gajanan");
        a1.setAge(22);

        Alien a2 = new Alien();
        a2.setId(2);
        a2.setName("Omkar");
        a2.setAge(22);

        Alien a3 = new Alien();
        a3.setId(2);
        a3.setName("Omkar");
        a3.setAge(22);

        Laptop l1 = new Laptop();
        l1.setLid(101);
        l1.setBrand("Dell");
        l1.setModel("11 Generation");
        l1.setRam(32);

        Laptop l2 = new Laptop();
        l2.setLid(102);
        l2.setBrand("HP");
        l2.setModel("12 Generation");
        l2.setRam(16);

        Laptop l3 = new Laptop();
        l3.setLid(103);
        l3.setBrand("Asus");
        l3.setModel("12 Generation");
        l3.setRam(64);

        l1.setAlien(Arrays.asList(a1, a2));
        l2.setAlien(Arrays.asList(a1));

        session.persist(a1);
        session.persist(a2);

        session.persist(l1);
        session.persist(l2);

```
