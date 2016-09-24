#powershell

:> sqlcmd -S "(localdb)\mssqllocaldb"

1> CREATE DATABASE todo;
2> GO
1> USE todo;
2> GO
1> CREATE TABLE tasks
2> (
3>   id INT IDENTITY (1,1),
4>   description VARCHAR(255)
5> );
6> GO

#Microsoft SQL Server Management Studio

enter the following in the Connect to Server window and hit Connect:

###Server type: Database Engine
###Server name: (localdb)\MSSQLLocalDB
###Authentication: Windows Authentication
