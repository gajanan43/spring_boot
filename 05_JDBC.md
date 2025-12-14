# JDBC:

```
package org.example;
import java.sql.*;  /------------Step1
import java.util.*;


public class Main {
    public static void main(String[] args) {
    /*
        import package
        load & register
        create connection
        create statement
        execute statement
        process the results
        close
     */

        String url = "jdbc:mysql://localhost:3306/mydb";
        String user = "root";
        String password = "mysql";
        // String query = "select * from student"; //Read-1
        // String query = "update student set id='4' where id='3'"; //Update-2
        // String query = "delete from student where id='3'"; //Delete-3
        String query = "INSERT INTO student(name, email) VALUES('Shivam','demo3@gmail.com')"; // Insert-4

        //2-----> (Optional) Explicit driver load:

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }

        //3---->  create connection
        try(Connection connection = DriverManager.getConnection(url,user,password)){
            System.out.println("✅ Connected to MySQL successfully!");

            //4-----> create statement
            try(Statement statement = connection.createStatement()){

                //5-----> execute statement

                //CRUD Operation
                // Read-1
                // ResultSet rs=statement.executeQuery(query);

                //Update-2 &
                //st.executeUpdate(query);

                //Delete-3 & Insert-4
                statement.execute(query);
                ResultSet rs=statement.executeQuery("select * from student");


                //6-----> process the results
                while (rs.next()) {
                    int id = rs.getInt("id");
                    String name = rs.getString("name");
                    String email = rs.getString("email");
                    System.out.println("id=" + id + " name=" + name + " email=" + email);

                }
            }
        }
        catch (SQLException e){
            System.out.println("❌ Connection failed!");
        }

    }
}

```




## 🧾 Code Explanation (Line by Line)

```java
package org.example;
```

➡️ Defines the package (like a folder name) your class belongs to — here it’s `org.example`.

---

```java
import java.sql.*;
import java.util.*;
```

➡️ Imports classes from the **JDBC (Java Database Connectivity)** API.
`java.sql.*` gives you access to `Connection`, `DriverManager`, `Statement`, `ResultSet`, etc.
`java.util.*` is imported but not used here — you can remove it safely.

---

```java
public class Main {
    public static void main(String[] args) {
```

➡️ Standard Java main class and entry point.
Everything inside `main()` runs when the program starts.

---

```java
/*
    import package
    load & register
    create connection
    create statement
    execute statement
    process the results
    close
 */
```

➡️ A helpful comment describing the **7 JDBC steps** — a good habit 👍.

---

```java
String url = "jdbc:mysql://localhost:3306/mydb";
String user = "root";
String password = "mysql";
String query = "select * from student ";
```

➡️ Here you set up:

* **`url`** → the JDBC connection URL:

  * `jdbc:mysql://` → protocol and database type
  * `localhost` → your database server (running on your own computer)
  * `3306` → default MySQL port
  * `mydb` → name of the database
* **`user`** and **`password`** → credentials to log into MySQL.
* **`query`** → SQL statement you want to run (`SELECT * FROM student`).

---

```java
try {
    Class.forName("com.mysql.cj.jdbc.Driver");
} catch (ClassNotFoundException e) {
    throw new RuntimeException(e);
}
```

➡️ This line **loads and registers** the MySQL JDBC driver.

* Before Java 6, you had to load the driver manually.
* In modern Java, this step is optional because it auto-loads when the dependency is added, but it’s still okay to keep it for clarity.
  If the class isn’t found (driver JAR missing), a `ClassNotFoundException` is thrown.

---

```java
try(Connection connection = DriverManager.getConnection(url, user, password)) {
```

➡️ **Creates the connection** to MySQL using `DriverManager`.

* `DriverManager.getConnection()` establishes the link between Java and your database.
* The `try (...)` syntax is called **try-with-resources** — it automatically **closes the connection** when the block ends (no need for `connection.close()` manually).

---

```java
System.out.println("✅ Connected to MySQL successfully!");
```

➡️ Prints a confirmation message when the connection is successful.

---

```java
try(Statement statement = connection.createStatement()) {
```

➡️ Creates a **Statement object** to send SQL queries to the database.
You can use:

* `Statement` → for simple static SQL
* `PreparedStatement` → for parameterized or repeated SQL queries (safer & faster)

---

```java
ResultSet rs = statement.executeQuery(query);
```

➡️ Executes the SQL query (`SELECT * FROM student`) and stores the results in a **ResultSet** object.

* `ResultSet` behaves like a cursor that points to the rows returned from the database.

---

```java
while (rs.next()) {
```

➡️ Moves the cursor to the next row.
Returns `true` if there *is* a row, otherwise `false`.
This loop continues until all rows are read.

---

```java
int id = rs.getInt("id");
String name = rs.getString("name");
String email = rs.getString("email");
```

➡️ Reads each column value from the current row using the column names from your table (`id`, `name`, `email`).

---

```java
System.out.println("id=" + id + " name=" + name + " email=" + email);
```

➡️ Prints the data nicely formatted for each record in the table.

---

```java
}
```

➡️ Ends the while loop (after all rows are printed).

---

```java
}
```

➡️ Ends the inner `try` block — the **Statement** automatically closes here.

---

```java
}
catch (SQLException e) {
    System.out.println("❌ Connection failed!");
}
```

➡️ This `catch` block runs if something goes wrong — for example:

* Invalid SQL
* Wrong username/password
* MySQL server not running

It prints an error message instead of crashing the program.

---

✅ **Summary of the Flow:**

| Step | Action           | Class Used                      |
| ---- | ---------------- | ------------------------------- |
| 1    | Load driver      | `Class.forName()`               |
| 2    | Connect to DB    | `DriverManager.getConnection()` |
| 3    | Create Statement | `Connection.createStatement()`  |
| 4    | Execute Query    | `Statement.executeQuery()`      |
| 5    | Process Results  | `ResultSet`                     |
| 6    | Close Resources  | Auto via `try-with-resources`   |

---

