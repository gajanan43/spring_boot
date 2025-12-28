# Lazy Fetch:

```

Transaction tx=  session.beginTransaction();
        session.persist(l1);
        session.persist(l2);
        session.persist(l3);
        session.persist(s1);
        session.persist(s2);

        tx.commit();



        session.close();

        Session session1=sf.openSession();
        Student s3=session1.find(Student.class,2);  //----------Lazy fetch (Bydefault)
        // System.out.println(s3);    //Ans2--------remove comment

        session1.close();


RESULT:

Ans1======================Hibernate: select s1_0.id,s1_0.age,s1_0.name from Student s1_0 where s1_0.id=?

But System.out.println(s3); remove then

Ans2===================Hibernate: select l1_0.Student_id,l1_1.lid,l1_1.brand,l1_1.ram from Student_Laptop l1_0 join Laptop l1_1 on l1_1.lid=l1_0.laptops_lid where l1_0.Student_id=?
                        Student{id=2, name='Omkar', age=23, laptops=[Laptop{lid=103, brand='Asus', ram=8}]}
```

# Eager:

```
Student.class

    @OneToMany(fetch = FetchType.EAGER)  //----------Eager Fetch 
    private List<Laptop> laptops;


Main.class

Transaction tx=  session.beginTransaction();
        session.persist(l1);
        session.persist(l2);
        session.persist(l3);
        session.persist(s1);
        session.persist(s2);

        tx.commit();



        session.close();

        Session session1=sf.openSession();
        Student s3=session1.find(Student.class,2);  //----------Lazy fetch Bydefault
        // System.out.println(s3);    

     session1.close();

RESULT:

Ans======================Hibernate: select s1_0.id,s1_0.age,s1_0.name,l1_0.Student_id,l1_1.lid,l1_1.brand,l1_1.ram from Student s1_0 left join Student_Laptop l1_0 on s1_0.id=l1_0.Student_id left join Laptop l1_1 on l1_1.lid=l1_0.laptops_lid where s1_0.id=?


```

