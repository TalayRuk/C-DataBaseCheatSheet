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
##### public static list<Task> GetAll()

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

#### Once we have the kitten object, we compare their names. If they're the same, we return true to indicate that they are the same kitten.

4. Need this method _to compare a kitten with an object of another class:_
#### add public override int GetHashCode()
public override int GetHashCode()
{
     return this.GetName().GetHashCode();
}

5. Once retrieve the List return from GetAll() We can retrieve the first & only saved in that list testTask. Then run the Equal() = assertion to the test that they are the same. Now we can add:
#### Add public void Save()
-

SQLCMD -S "(localdb)\mssqllocaldb" -d hair_style

-add stylist database table
-backup & restore hair_salon_tests
-add stylist.cs
-add stylistTests.cs
-add foreign key 1> ALTER TABLE tasks ADD stylist_id INT; 2> GO *b/c 1 stylist can have many Client -one to many
-add
