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

use this link for below : https://www.learnhowtoprogram.com/c/c-database-basics/one-to-many-relationships-in-sql-for-c

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

#### 13. Add Foreign KEY
_so that a task can belong to a category. To do this we will need to save the task's category ID into our task Table. **The foreign key** = a column that connects one table to another using its ID_
- log in to our database again with **SQLCMD -S "(localdb)\mssqllocaldb" -d hair_salon**

1> ALTER TABLE tasks ADD stylist_id INT;
2> GO

#### 14. Also need to BACKUP hair_salon & RESTORE *hair_salon_test*

#### 15. GO BACK to TaskTest.cs & add categoryId (1) as second argument when initializing new Task(s).
**_write a unit test for our Task class that ensures we are saving our categoryId into our tasks table. Now that we need to pass the constructor a categoryId, we'll need to update all of our tests to include a category ID when initializing a task_**

#### 16. Modify in Task.cs to include a categoryId in Task class constructor.
-  now that we pass in 1 as the addition argument for **new Tasks.** We are going to modify the Task class constructor to add **categoryId** as its addition argument.

  1. **add int CategoryId to Task constructor**

  2. **add Getter & Setter for CategoryId()**

- 3. **add bool categoryEqaulity in public override also in return add (categoryEqaulity)**

- 4. **GetAll() at rdr.Read()**
      - add int taskCategoryId = rdr.GetInt32(2);
      - add taskCategoryId to new Task

- 5. **Save() at SqlCommand**
      - add @taskCategory);, conn);
      - add SqlParameter categoryIdParameter**
      - _SqlParameter categoryIdParameter = new SqlParameter();
    categoryIdParameter.ParameterName = "@TaskCategoryId";
    categoryIdParameter.Value = this.GetCategoryId();_
     - Add cmd.Parameters.Add(categoryIdParameter);

- 6. **public static Task Find(int id)**     
     - Below SqlDataReader rdr
        - Add int foundTaskCategoryId = 0;
     - Below while(rdr.Read())
        - foundTaskCategoryId = rdr.GetInt32(2);
     - At Task foundTask  = new Task(Add foundTaskCategoryId)  

#### 17. in CategoryTest.cs add void Test RetrievesAllTasks withCategory()
---
    [Fact]
    public void Test_GetTasks_RetrievesAllTasksWithCategory()
    {
      Category testCategory = new Category("Household chores");
      testCategory.Save();

      Task firstTask = new Task("Mow the lawn", testCategory.GetId());
      firstTask.Save();
      Task secondTask = new Task("Do the dishes", testCategory.GetId());
      secondTask.Save();


      List<Task> testTaskList = new List<Task> {firstTask, secondTask};
      List<Task> resultTaskList = testCategory.GetTasks();

      Assert.Equal(testTaskList, resultTaskList);
    }---

#### 18. in Category.cs: Create GetTask() to get test above passing.
  _This is very similar to our GetAll() methods. We take static off of the method declaration so we can call it on our Category instances, and then use that instance's id as the reference for our SQL query. We can then return our SqlCommand result directly as a List<Task>._s           
---
    public List<Task> GetTasks()
    {
      SqlConnection conn = DB.Connection();
      conn.Open();

      SqlCommand cmd = new SqlCommand("SELECT * FROM tasks WHERE category_id = @CategoryId;", conn);
      SqlParameter categoryIdParameter = new SqlParameter();
      categoryIdParameter.ParameterName = "@CategoryId";
      categoryIdParameter.Value = this.GetId();
      cmd.Parameters.Add(categoryIdParameter);
      SqlDataReader rdr = cmd.ExecuteReader();

      List<Task> tasks = new List<Task> {};
      while(rdr.Read())
      {
        int taskId = rdr.GetInt32(0);
        string taskDescription = rdr.GetString(1);
        int taskCategoryId = rdr.GetInt32(2);
        Task newTask = new Task(taskDescription, taskCategoryId, taskId);
        tasks.Add(newTask);
      }
      if (rdr != null)
      {
        rdr.Close();
      }
      if (conn != null)
      {
        conn.Close();
      }
      return tasks;
    } ---
