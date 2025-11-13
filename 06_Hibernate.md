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

