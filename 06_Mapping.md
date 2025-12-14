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

```
