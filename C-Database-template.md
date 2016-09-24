Steps

Before we start writing our code, we'll need to open SQLCMD, create a database named todo, and make a table called tasks with a primary key id and a varchar column called description:

SQLCMD -S "(localdb)\mssqllocaldb"

1> CREATE DATABASE hair_salon; 2> GO 1> USE hair_salon; 2> GO 1> CREATE TABLE Client 2> ( 3> id INT IDENTITY (1,1), 4> c_name VARCHAR(255), c_phone INT 5> ); 6> GO

Create project files .. Db class in database.cs -then create backup from hair_style database -create restore hair_style_test database

Now that we've configured our application to work with a database and run tests on it, Start on the actual app. set up our test file using Xunit; using System.Collections.Generic; the using System; using System.Data; using System.Data.SqlClient;

namespace HairSalon { public class ClientTest : IDisposable { public ClientTest() { DBConfiguration.ConnectionString = "Data Source=(localdb)\mssqllocaldb;Initial Catalog=hair_salon_test. Cobb. ;Integrated Security=SSPI;"; }

public void Dispose()
{
  Task.DeleteAll();
}
}
}

-In our ClientTest constructor, we set DBConfiguration.ConnectionString to our hair_salon_test database, which overrides the DBConfiguration.ConnectionString we set in Startup.cs.

1. first spec, which will test that we have an empty database to start testing:
#### testing the static Task.GetAll() method.
 - _*Here we will want to pull out all of the tasks from the database into a list. Since we haven't saved anything yet, the list will be empty._
 - our first spec we are testing the static Task.GetAll() method then we will pull out all the tasks from the database into a list & since we haven't saved anything yet the list will be empty.

2. Start writing Client.cs: write class & constructor, Getter & Setter
#### add GetAll() method to Task class
#### public static list<Task> GetAll()
then
#### add DeleteAll() method to Task class

3. B4 we save tasks to the database, need to override to tell
database that both object are equally the same
#### write spec to test for override the Equals() method
public override bool Equals(System.Object otherKitten)
{
  if (!(otherKitten is Kitten))
  {
    return false;
  }
  else
  {
    Kitten newKitten = (Kitten) otherKitten;
    return this.GetName().Equals(newKitten.GetName());
  }
}
_the default Equals method accepts any type of object, when we override it, we need to use (System.Object otherKitten) for the argument to match the method signature. Then, we turn the object into a Kitten object with Kitten newKitten = (Kitten) otherKitten;. When we change an object from one type to another, it's called type casting._

**Once we have the kitten object, we compare their names. If they're the same, we return true to indicate that they are the same kitten.**

4. Need this method _to compare a kitten with an object of another class:_
#### add public override int GetHashCode()
public override int GetHashCode()
{
     return this.GetName().GetHashCode();
}

5. Once retrieve the List return from GetAll() We can retrieve the first & only saved in that list testTask. Then run the Equal() = assertion to the test that they are the same. Now we can add:
#### Add public void Save()
- In the **cmd** variable, we use the **parameter placeholder** *@TaskDescription*. We want to use use parameter placeholders whenever we are entering data that a user enters. Information stored to a parameter is treated as field data and note part of the SQL statement, which helps to **protect our application from SQL injection(=hack web applications)!**
*need to go back & read about parameter https://www.learnhowtoprogram.com/c/c-database-basics/to-do-list-with-databases-part-4-sql-queries-with-parameters*

6. When task is saved into database, it's assigned a unique ID. We want to make sure that when the task objects that we instantiate to have the same ID as the column they are built from. Also when we save the object to the database, we want it to have the same ID that is assigned to it in the database.
#### Write a test spec for Save() to test this behavior.

7. now we have save to ID
#### Modify override Equal() to account for ID by adding idEquality

8. we need to be able to find objects we have saved into the database.
#### Test: Find() .. from the save() & getId()
*The line Task.Find(testTask.GetId()) uses the task's id to query the database and construct a new object from the information stored in that row.
To make it pass*

9. after the test
#### Write Find() method in Task.cs
Here we are using a SELECT query using WHERE id = @TaskId. We set @TaskId equal to the id that we pass into the Find() method, and convert it to a string so that it can be used in the query string. Then we read the result of the query to create a new Task named foundTask and return it.

10. Now we need to add TABLE stylist to hair_style db
#### SQLCMD -S "(localdb)\mssqllocaldb" -d hair_salon
-add stylist database table since it's a one to many relation; the 1 stylist can has many clients but a client can only have 1 stylist thus client is act as task & stylis is act as category (in hair_salon project, add stylists instead of categories)
1> CREATE TABLE categories (id INT IDENTITY (1,1), s_name VARCHAR(255));
2> GO

11. then Recreate our **hair_salon_test** database to use the new schema, & including the stylists table.
#### Backup **hair_salon** & restore **hair_salon_test**

#### 12. Create StylistTests.cs
**adding hair_salon_test to DBConfiguration**
- do the same as ClientTests.cs

13.

-add foreign key
1> ALTER TABLE tasks ADD stylist_id INT; 2> GO *b/c 1 stylist can have many Client -one to many
-add
