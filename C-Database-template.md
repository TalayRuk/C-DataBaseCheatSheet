Steps

Before we start writing our code, we'll need to open SQLCMD, create a database named todo, and make a table called tasks with a primary key id and a varchar column called description:

SQLCMD -S "(localdb)\mssqllocaldb"

1> CREATE DATABASE hair_salon; 2> GO 1> USE hair_salon; 2> GO 1> CREATE TABLE clients 2> ( 3> id INT IDENTITY (1,1), 4> c_name VARCHAR(255), c_phone INT 5> ); 6> GO

Create project files .. Db class in database.cs -then create backup from hair_style database -create restore hair_style_test database

Now that we've configured our application to work with a database and run tests on it, Start on the actual app. set up our test file using Xunit; using System.Collections.Generic; the using System; using System.Data; using System.Data.SqlClient;

namespace HairSalon { public class ClientsTest : IDisposable { public ClientsTest() { DBConfiguration.ConnectionString = "Data Source=(localdb)\mssqllocaldb;Initial Catalog=hair_salon_test. Cobb. ;Integrated Security=SSPI;"; }

public void Dispose()
{
  Task.DeleteAll();
}
}
}

-In our ClientsTest constructor, we set DBConfiguration.ConnectionString to our hair_salon_test database, which overrides the DBConfiguration.ConnectionString we set in Startup.cs.

Now let's write our first spec, which will test that we have an empty database to start testing
testing the static Task.GetAll() method. *Here we will want to pull out all of the tasks from the database into a list. Since we haven't saved anything yet, the list will be empty.
SQLCMD -S "(localdb)\mssqllocaldb" -d hair_style

-add stylist database table
-backup & restore hair_salon_tests
-add stylist.cs
-add stylistTests.cs
-add foreign key 1> ALTER TABLE tasks ADD stylist_id INT; 2> GO *b/c 1 stylist can have many clients -one to many
-add
