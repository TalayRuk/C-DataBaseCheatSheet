Microsoft SQL Server Management Studio (SSMS). SSMS is an integrated environment that provides a number of tools and an interface for managing and developing all parts of SQL Server. Start up SSMS, and enter the following in the Connect to Server window and hit Connect:

Server type: Database Engine
Server name: (localdb)\MSSQLLocalDB
Authentication: Windows Authentication

Find the Object Explorer window, which displays a tree view of all the database objects in a server. Expand the (localdb)\MSSQLLocalDB, and you should see folders for Databases, Security, Server Objects, Replication, and Management.

Let's create a database with tables and data
1. Open your terminal and create a database named ToDoList.
2. In SSMS, right click on the server instance, (localdb)\mssqllocaldb… and choose Refresh. The ToDoList database will show up in the Databases folder in the Object Explorer.
3. Expand the ToDoList database. There are a number of folders which contain the settings for our database. We're not going to worry about most of them, but expand the Tables folder. This folder gives us access to our individual tables. Since this is a new database, there aren't any tables yet.
4. Let's create a table from within our terminal. Using what we know about creating tables, let's create a table called tasks within our new ToDoList database. For now, let's give it one column, called id .
1> CREATE TABLE tasks (id INT IDENTITY (1,1))
2> GO

5. When we refresh our tables folder, see that there is a new item called dbo.tasks. This is our tasks table. We can even edit it from within SSMS. Right-click the file dbo.tasks and choose Design.
6. ***_it's a good idea to continue using our terminal to create our tables, as it gives us control over the specifics of our table creation._***
7. Let' create a new column for a description with the data type VARCHAR(50).
8. Finally, let's look at the data in our table. Right-click on dbo.tasks and select Edit Top 200 Rows.... This gives us a list of the data in our database. Let's use the SSMS interface to enter some data manually. Notice that the id is automatically generated. This is because of the IDENTITY property we set when we created the column.

Once we've added some data, let's go back to our terminal and see what we find in our database.

1> USE ToDoList;
2> GO
3> SELECT * FROM tasks;
4> GO
id                        description
---------------------  ------------------------------
                        1  Wash the car
                        2  Prepare dinner

(2 rows affected)
It's important to see that the database we create in our terminal are available to other programs. We'll be using SSMS to manage our databases in the future. The program is a very powerful tool and there are many great resources for it online.
