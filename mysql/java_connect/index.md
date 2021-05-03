# Connecting to MySQL from a Java application

## Obtaining the mysql-connector library

To connect to a MySQL database from a Java application, we need a module that handles the connection for us.

Go to [https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/) and download the *platform independent* connector.
You want the zip-file, not the `tar.gz` file.

![]({{ site.url }}/mysql/java_connect/images/download.png)


Unzip the connector and copy the file `mysql-connector-java-8.0.24.jar` to the folder where you store your Java libraries. (This should probably be the same place as where you store the JavaFX libraries).

## Setting up an example Java project


In IntelliJ, create a new project.

Go to the *Module Settings*. In the *Dependencies* pane, add the `mysql-connector-java-8.0.24.jar` you just downloaded.

![]({{ site.url }}/mysql/java_connect/images/add_module_1.png)
![]({{ site.url }}/mysql/java_connect/images/add_module_2.png)


Open MySQL workbench. In the "welcome"-window, right click the database instance you want to connect to and select "Copy JDBC Connection String to Clipboard".
We need to copy this into our Java program, in order to connect to our database.

![]({{ site.url }}/mysql/java_connect/images/jdbc_string.png)

Write off (or copy/paste) the code below into your project:

If you did not set a password in the MySQL database, replace `password` with `null` in the `DriverManager.getConnection` call.

``` java
package dk.diku.systemudvikling2021;

import java.sql.*;

public class Main {
    public static void main(String[] args) {
	    String url = "the JDBC connection string you get from MySQL Workbench";
        String password = "your password if you set one";

        String query = "SELECT VERSION()";

        try (Connection con = DriverManager.getConnection(url, null, password);
             Statement st = con.createStatement();
             ResultSet rs = st.executeQuery(query)) {

            if (rs.next()) {
                String version = rs.getString(1);
                System.out.println("MySQL version: " + version);
            }

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }
}
```

If the connection is set up correctly, your console should print the version number of your MySQL database.


If you followed the PetStore example, you can try printing out the animals we inserted:


``` java
package dk.diku.systemudvikling2021;

import java.sql.*;

public class Main {
    public static void main(String[] args) {
	    String url = "the JDBC connection string you get from MySQL Workbench";
        String password = "your password if you set one";

        String query = "SELECT * FROM PetStore.ANIMALS";

        try (Connection con = DriverManager.getConnection(url, null, password);
             Statement st = con.createStatement();
             ResultSet rs = st.executeQuery(query)) {

            while (rs.next()) {
                int id = rs.getInt(1);
                String type = rs.getString(2);
                Date date = rs.getDate(3);
                String name = rs.getString(4);
                System.out.println("id: " + id + " | type: " + type + " | birthdate: " + date + " | name: " + name);
            }

        } catch (SQLException ex) {
            System.out.println("Crap!");
            System.out.println(ex.getMessage());
        }
    }
}
```
