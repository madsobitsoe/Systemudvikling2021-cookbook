# Ensuring MySQL is set up correctly through MySQL Workbench

Open MySQL Workbench. In the main window, under "MySQL Connections", you should see one database:

![]({{ site.url }}/mysql/first_exercises/images/setup_1.png)




# Users in MySQL

For the course it will be okay to use the root user for everything.

In a real production environment, you would create specific users and designate/administer their privileges for specific databases.


# Terminology/Glossary

| Term        | Meaning                                                                                                                                                             |
|:-----------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SQL         | Structured Query Language.                                                                                                                                          |
| Query       | A command/program to be executed in our DBMS system.                                                                                                                |
| DBMS        | DataBase Management System                                                                                                                                          |
| MySQL       | A Database Management System                                                                                                                                        |
| Database    | A collection of one or more tables with relations between them. Think of it as a very advanced excel-workbook with multiple interconnected sheets.                  |
| Table       | A collection of data records. Corresponds to a single sheet in Excel                                                                                                |
| Row         | A single line from a table.                                                                                                                                         |
| Primary Key | A unique identifier for a row. Typically a number - or something like a CPR.                                                                                        |
| Foreign Key | An identifier that links a row in one table, to possibly many rows in another table. (One-to-many relationships)                                                    |
| DDL         | Data Definition Language. Queries that create/delete/modify databases and tables                                                                                    |
| DML         | Data Manipulation Language. Queries that create/delete/modify data, e.g. inserts data into tables, selects (fetches) data from tables, modifies data in tables etc. |
| Schema      | A Database. A collection of tables.                                                                                                                                                                    |




# Creating a database

To familiarize ourselves with MySQL and MySQL workbench, we will create a simple database, to manage a pet store.

To create our first database, start by clicking the "Create a new schema in the connected server"-button:

![]({{ site.url }}/mysql/first_exercises/images/new_schema.png)


Fill in a name for our new database, then click the small "apply"-button. I call my database "PetStore":

![]({{ site.url }}/mysql/first_exercises/images/new_schema_1.png)

Now a window with a small piece of code should pop up.
This is an SQL query (DDL to be exact), that will create a new database (schema).
Click Apply to execute the query.
![]({{ site.url }}/mysql/first_exercises/images/new_schema_2.png)


Now click the "Schemas"-tab. You should be able to see our newly created database, PetStore!

![]({{ site.url }}/mysql/first_exercises/images/new_schema_3.png)


We can not do much with just a database. We need tables, in order to store data.


# Creating a table in our database





<!-- 2) Create a new user -->

<!-- Currently, only the root user (administrator) for the entire MySQL program exists. -->
<!-- We will now create a user -->

<!-- Click the one database from the screenshot above. -->


<!-- <\!-- Fill in steps... -\-> -->



<!-- <\!-- # Connecting from Java - Næste uges exercise -\-> -->
