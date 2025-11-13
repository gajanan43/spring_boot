# Hibernate:

- Hibernate is ORM(Object Relation Mapping) Framework.
- It is solve the problem of jdbc beacuse there in write lot of qurey.

## Setup: 

1) Create a project into maven
2) Add MYSQL dependencies into pom.xml file
3) Create a class file for DB(Write a structure of DB)
4) Create a hibernate.cfg.xml file inside resources folder(write a connection property for DB)
5) Write a main file(CURD operation)


## Method in Hibernate:
1) persist -> To insert data into the DB
2) merge  -> Update the data in DB(if data is new then this work as insert query)
3) remove  -> Delete the data from DB
4) get  -> View data from DB
5) find  -> Find the specific data from DB

```
package org.example;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;


public class Main {
    public static void main(String [] args) {

        Student s1=new Student();
//        s1.setId(4);
//        s1.setName("You");
//        s1.setEmail("demo4@gmail.com");

//        Student s2=null; // Create a null object to read a specific row

//        Configuration cfg=new Configuration();
//        cfg.addAnnotatedClass(org.example.Student.class);
//        cfg.configure();

        SessionFactory sf= new  Configuration()
                .addAnnotatedClass(org.example.Student.class)
                .configure()
                .buildSessionFactory();   // cfg.buildSessionFactory();

        Session session=sf.openSession();
//      s2=session.get((Student.class,4);  //Read Query
        s1=session.find(Student.class,4); //find object to the delete data realted object
        Transaction tx= session.beginTransaction();

//        session.merge(s1); // Update & insert Query
        session.remove(s1); // Delete query
//        session.persist(s1);    // Insert Query
        tx.commit();
        sf.close();
        session.close();
        System.out.println(s1);
//      System.out.println(s2);
    }
}

```


## Notations in Hibernate:
1) Entity
2) Id
3) Cloumn
4) Transit
5) Embeddable

