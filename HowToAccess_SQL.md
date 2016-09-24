Lesson Weekend
C# C# database basics SQL basics for C#

We can access the SQL server on our computer using the SQLCMD utility. This is a program that allows us to interact with our databases using PowerShell. To access the SQLCMD shell at Epicodus, open up PowerShell and run:

> sqlcmd -S "(localdb)\mssqllocaldb"
You are now logged into your local SQL server. You should have a prompt that looks something like this:

1>
SQL server manages multiple databases. In order to create content or make changes in a database, we need to navigate into that particular database from SQLCMD. To determine which database we are currently connected to, type:

1> SELECT DB_NAME();
2> GO
You will see that the current database is listed as master. This is because we haven’t told SQL where we want to begin, and this is the default database. Let's create a real database of our own. In the shell, type:

1> CREATE DATABASE test_database;
2> GO
This is our first SQL statement. We just created a database named test_database. SQL keywords aren't case sensitive, but are conventionally typed in all caps. All SQL statements must end with a semicolon. The word GO executes any SQL statements that come before it.

Let's list out all of the databases in our SQL server:

1> SELECT name FROM Sys.Databases;
2> GO
There is a database used to hold metadata for other databases, master. You shouldn't ever delete this. And now you should have a database called test_database.

To create tables and columns in our database, we need to connect to it:

1> USE test_database;
2> GO
If we run SELECT DB_NAME(); now, we see that we're connected to the test_database.

1> CREATE TABLE contacts
2> (
3>   name VARCHAR(255),
4>   age INT,
5>   birthday DATETIME
6> )
7> GO
Hitting Enter after each line does not actually do anything; the above statement can also be written as:

1> CREATE TABLE contacts (name VARCHAR(255), age INT, birthday DATETIME)
2> GO
Nothing actually happens until we say > GO.

This statement creates a table called contacts with a column for name, age, and birthday. Just like properties of a C# class, SQL columns have a data type. In this case, name has the data type varchar, which means a varying number of characters (this is more or less the SQL equivalent of a string); age is an int, or integer; and birthday is a datetime, which includes a date and time. Some data types have restrictions and requirements; here, varchar takes a maximum length, which we've set to 255 characters. Most of the time, 255 is a good default maximum length for varchar. If you do not specify a maximum length, SQL will set it to 1.

SQL has many data types, but here are the most common ones you'll use: int, float, varchar, text (for long blocks of text), timestamp, datetime, and boolean. For a full list, here is a handy reference.

To list the columns in our new table, use the following statement:

1> SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'contacts';
2> GO
To see a list of all the tables in the database, type

1> SELECT name FROM sysobjects WHERE xtype='u';
2> GO
This syntax comes from the sysobjects class, which you can read more about here.

If you mess something up and need to delete a table, type DROP TABLE table_name;, where table_name is the name of the table.

Here's how to add a column to a table:

1> ALTER TABLE contacts ADD favorite_color VARCHAR(255);
2> GO
Drop a column like this:

1> ALTER TABLE contacts DROP COLUMN favorite_color;
2> GO
It's a good idea to always have a unique, automatically-incrementing ID number for each record in your database. Here's how to add such a field:

1> ALTER TABLE contacts ADD id INT IDENTITY(1,1);
2> GO
The IDENTITY keyword is used when we want a column to be auto-incrementing. While we are not declaring this column a primary key, it gives each row a unique identifier that we can reference in a similar way.

In the example above, the starting value for IDENTITY is 1, and it will increment by 1 for each new record. So if we wanted to specify that the id column should start at value 10 and increment by 5, we should change it to IDENTITY(10,5).

Now that we have tables with columns, let's put some data in them:

1> INSERT INTO contacts (name, age, birthday) VALUES ('Jane', 27, '1985-09-22');
2> GO
Notice how we need to put quotes around varchars and datetimes, but not around ints. Also, remember that with SQL, we need to use single quotes - double quotes won't work.

Often, we'll need to get back the ID from an insert, so that we know how to find the row we just inserted. Here's how to return the ID from an INSERT statement:

INSERT INTO contacts (name, age, birthday) OUTPUT INSERTED.id VALUES ('Wes', 43, '1969-05-01')
Now that we have data in our database, let's get it out. Reading data from a database is called querying. Here's a query for you to try:

1> SELECT name FROM contacts;
2> GO
This gives us a list of all the names in our contacts table.

You can select multiple columns from a table:

1> SELECT name, birthday FROM contacts;
2> GO
If we want to select all of the columns from a table, there's a shortcut:

1> SELECT * FROM contacts;
2> GO
We can limit your SELECTs to only return rows that match certain criteria:

1> SELECT * FROM contacts WHERE age >= 18;
2> GO
This gives us all of the adults in contacts.

A WHERE clauses accepts the following operators: =, !=, >, <, >=, <=, BETWEEN, LIKE, and IN. We can also prepend any operator with NOT. Here are examples of the last couple operators:

_##> SELECT * FROM contacts WHERE age BETWEEN 13 AND 17;  
> SELECT * FROM contacts WHERE name BETWEEN 'Judith' AND 'Wilma';
> SELECT * FROM contacts WHERE name LIKE 'We%';
> SELECT * FROM contacts WHERE birthday IN ('1969-01-01', '1999-01-01');
For the LIKE operator, the % is a wildcard, meaning it can stand for any number of any characters.

What if we need to change data in our database? There are a lot of fancy ways to do this, but let's focus on the simplest and most common: select a record by its primary key, and update its data:

1> UPDATE contacts SET name = 'Wes Anderson' WHERE id = 1;
2> GO
Now, let's delete a record:

1> DELETE FROM contacts WHERE id = 1;
2> GO
We're done playing around with this database now, so let's drop it. You cannot drop a database you're currently connected to. So make sure to switch back to master by first running USE master:

1> DROP DATABASE test_database;
2> GO
Finally, to quit SQLCMD, just run:

1> quit
