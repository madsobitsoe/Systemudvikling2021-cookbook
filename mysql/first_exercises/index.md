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

In the "Schemas"-tab, unfold the PetStore-database, right-click the "tables"-entry and select "Create Table..".

![]({{ site.url }}/mysql/first_exercises/images/new_table_1.png)

Now you will see a GUI-interface that helps us create a table, i.e. generates the DDL-SQL we need to create our table.

We need to store information on the animals available in our pet-store.
We need a primary key, ie. a unique identifier for each animal in the store.
Call it `idAnimals` and make sure the `PK` column, the `NN` column and the `UQ` column are checked.
`PK` means `Primary Key`, `NN` means `NOT NULL`, i.e. that this fields *needs* to have data, and `UQ` means unique, i.e. that only one row in the table can contain a specific key.


We also need to store the type of animal, so we can count how many dogs we have for sale as well as figure out if we have run out of *python*!
We can use the `MySQL`-Datatype `VARCHAR` for this. VARCHAR is similar to a string in java/python. We need to decide the maximum length of the string. 45 is the default and should be fine for our use. Check the `NN` column, as this information is needed for all animals.

We also need to store the birthdate of each animal. Customers would probably like to know the age of their new pet.
We can use the `MySQL`-Datatype `DATE` for this. Check the `NN` column, as this information is needed for all animals.
Last but not least, an animal can have a name.
We can again use the `VARCHAR`-datatype for this. If an animal is born in the store, we don't give it a name - so do not check the `NN` column.



Click the small `apply` button when you are ready.
![]({{ site.url }}/mysql/first_exercises/images/new_table_2.png)


MySQL Workbench will now show you a piece of code, which again is SQL-DDL. Read the code and see if you can make some sense of what it does. You should be able to somewhat recognize the table we just designed. Click the large `Apply`-button to create the table.

![]({{ site.url }}/mysql/first_exercises/images/new_table_3.png)


In the "Schema"-tab, you can now unfold the "Table"-entry and should see our new table.
You can unfold the "Columns"-entry and should see the columns we created in our new table.

![]({{ site.url }}/mysql/first_exercises/images/new_table_4.png)


# Inserting data into our table


We are ready to fill our store with animals.
So far, we have only used SQL-DDL - the Data Definition Language - to create our database(schema) and our table.

Now we will use SQL-DML - the Data Manipulation Language - to create entries in our database.

To begin with we have 5 animals.

| Type  | BirthDate (YYYY/MM/DD) | Name                         |
|:------|:-----------------------|:-----------------------------|
| Dog   | 2020/05/01             | Good Boi                     |
| Cat   | 2015/12/24            | Santa Claws                  |
| Snake | 2017/02/03            | Sir Hiss                     |
| Cat   | 1950/03/14            | Meowser, destroyer of worlds |
| Dog   | 2020/05/01            | Goodest Chunky Boi           |


We can insert these into our database, by writing an *INSERT-statement*.

Right-click the animals table and choose "Send to SQL-Editor" and "Insert Statement".

![]({{ site.url }}/mysql/first_exercises/images/insert_1.png)

This gives us a template for inserting data into our table.
We adjust the template, so we can register the dog "Good Boi", then click the lightning icon to run our query.
The bottom area of the window gives us the status (return code!) of the queries we run. If we did everything correct, we should see a green arrow.

![]({{ site.url }}/mysql/first_exercises/images/insert_2.png)



Before we insert the 4 remaining animals, let's try to look at the data we just inserted.

We can do this with a *SELECT-statement*.
Delete the insert statement and write `SELECT * FROM PetStore.Animals;`.
Click the lightning icon to run your query. You should now see the *Good Boi* we just inserted.


![]({{ site.url }}/mysql/first_exercises/images/insert_3.png)

Wonderful!

Repeat this process to insert the remaining 4 animals, and/or any other animals you want in your pet-store.


# Select statements - checking what we have in store












<!-- 2) Create a new user -->

<!-- Currently, only the root user (administrator) for the entire MySQL program exists. -->
<!-- We will now create a user -->

<!-- Click the one database from the screenshot above. -->


<!-- <\!-- Fill in steps... -\-> -->



<!-- <\!-- # Connecting from Java - Næste uges exercise -\-> -->
