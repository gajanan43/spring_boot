# JDBC:

```
package org.example;
import java.sql.*;
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
        String query = "select * from student ";
        //2 (Optional) Explicit driver load:

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }

        //3  create connection
        try(Connection connection = DriverManager.getConnection(url,user,password)){
            System.out.println("✅ Connected to MySQL successfully!");

            //4 create statement
            try(Statement statement = connection.createStatement()){
                //5 execute statement
                ResultSet rs=statement.executeQuery(query);
`               // System.out.println(rs.next());

                //6 process the results
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
