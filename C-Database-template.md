Steps

*...check out conventions in database naming:https://www.learnhowtoprogram.com/c/c-database-basics/conventions-in-database-naming*

##See this link to Migrate databases w/SSMS (to export & import database) https://www.learnhowtoprogram.com/c/c-database-basics/migrating-databases-with-ssms

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
    }
---

#### 18. in Category.cs: Create GetTask() to get test above passing.
  _This is very similar to our GetAll() methods. We take static off of the method declaration so we can call it on our Category instances, and then use that instance's id as the reference for our SQL query. We can then return our SqlCommand result directly as a List<Task>._

---
    public List<Task> GetTasks() ...
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
    }
---

refer to this link for below : https://www.learnhowtoprogram.com/c/c-database-basics/nancy-with-databases

#### 19. in a Module\HomeModule.cs: **Add RESTful Route w/Nancy**
-  https://www.learnhowtoprogram.com/c/web-apps-with-nancy/restful-routes-with-nancy

---
    using System.Collections.Generic;
    using Nancy;
    using Nancy.ViewEngines.Razor;

    namespace ToDoList
    {
      public class HomeModule : NancyModule
      {
        public HomeModule()
        {
          Get["/"] = _ => {
            List<Category> AllCategories = Category.GetAll();
            return View["index.cshtml", AllCategories];
          };
        }
      }
    }
---

#### 20. Add Views\index.cshtml
---
    <h1>To Do List</h1>
    <h4><a href="/categories">View Categories</a></h4>
    <h4><a href="/tasks">View Tasks</a></h4>
---

#### 21. Now run >dnx kestrel from the shell
- to make sure everything runs smoothly

#### 22. Add Views\categories.cshtml
---
    <h1>All categories</h1>
    @if (@Model.Count == 0)
    {
      <p>There are no categories.</p>
    }
    <ul>
      @foreach (var category in Model)
      {
        <li><a href="/categories/@category.GetId()">@category.GetName()</a></li>
      }
    </ul>
    <a href="/categories/new">Add new category</a>
---

#### 23. Add Views\tasks.cshtml
---
    @if (@Model.Count == 0)
    {
      <p>No tasks have been added yet!</p>
    }
    else
    {
      <h1>Here is your list of tasks:</h1>
      <ul>
        @foreach (var task in Model)
        {
          <li>@task.GetDescription()</li>
        }
      </ul>
      <form action="/tasks/delete" method="post">
        <button type="submit">Clear all tasks</button>
      </form>
    }
    <p><a href="/tasks/new">Add a new task</a></p>
    <p><a href="/">Return home</a></p>
---

#### 24. at HomeModule.cs Add corresponding routes for added .cshtml
- Now, when we restart our server and visit the /tasks page we will see our tasks and categories. That is, we would if any existed. Since we haven't entered any tasks or categories, let's build out our functionality to create them.
- Let's start with the routes again.
---
    Get["/categories/new"] = _ => {
        return View["categories_form.cshtml"];
      };
      Post["/categories/new"] = _ => {
        Category newCategory = new Category(Request.Form["category-name"]);
        newCategory.Save();
        return View["success.cshtml"];
      };
      Get["/tasks/new"] = _ => {
        List<Category> AllCategories = Category.GetAll();
        return View["tasks_form.cshtml", AllCategories];
      };
      Post["/tasks/new"] = _ => {
        Task newTask = new Task(Request.Form["task-description"], Request.Form["category-id"]);
        newTask.Save();
        return View["success.cshtml"];
      };    
---
- we're creating a route to GET the form view, and a POST route the form view can call when we submit our new task or category.

#### 25. Create our Views\categories_form.cshtml,
**CATEGORIES_FORM.CSHTML**
---
    <form action="/categories/new" method="post">
      <label for="category-name">Category name</label>
      <input id="category-name" name="category-name" type="text">
      <button type="submit">Add</button>
    </form>
---

#### 26. create Views\tasks_form.cshtml
**TASK_FORM.CSHTML**
---
    <h1>Add a new task</h1>
    <form action="/tasks/new" method="post">
      <label for="task-description">Task description:</label>
      <input id="task-description" name="task-description" type="text">
      <select name="category-id">
        @foreach (var category in Model)
        {
          <option value="@category.GetId()">@category.GetName()</option>
        }
      </select>
      <button type="submit">Add new task</button>
    </form>
    <p><a href="/">Back home</a></p>
    <p><a href="/tasks">View all tasks</a></p>
---

### 27. create Views\success.cshtml
**SUCESS.CSHTML**
---
    <h1>Success!</h1>
    <a href="/">Return home</a>
---

_If we think of the app as a tree, it has two branches, one that represents tasks and one that represents categories. They overlap in some places, and grow together in some places, but basically at any given time the user will choose to either deal with tasks or categories. As you see, we're building out our tasks and categories branches at the same time, being careful to create our routes and then our views._

**This is only one order of creating these routes and views. For a given project, it may be more organized to build out one entire branch, then go back to the beginning and build out our other branches, it simply depends on the project, and how we like to work. Either way, it's important to have a plan, as it helps us stay organized, and identify the next step if we are unsure how to proceed.**

#### 28. create Views\cleared.cshtml
**CLEAR.cshtml**
---
    <h3>Tasks cleared!</h3>
    <a href="/">Back home</a>
---

#### 29. create Routes to delete all tasks, and a route that displays an individual category.
**Back to HomeModule.cs**
---
    ...
      Post["/tasks/delete"] = _ => {
        Task.DeleteAll();
        return View["cleared.cshtml"];
      };
      Get["/categories/{id}"] = parameters => {
        Dictionary<string, object> model = new Dictionary<string, object>();
        var SelectedCategory = Category.Find(parameters.id);
        var CategoryTasks = SelectedCategory.GetTasks();
        model.Add("category", SelectedCategory);
        model.Add("tasks", CategoryTasks);
        return View["category.cshtml", model];
      };
    ...
---
- _The Post["/tasks/delete"] route will allow us to delete all our existing tasks, and display a page telling us that the tasks have been cleared. Meanwhile, the Get["categories/{id}"] route will populate a single category along with its related tasks._

#### 30. create category.cshtml
- Let's review how this is done. We've placed the {id} into the route name. This is taking the id of the category we've selected, and added it to the route parameters as the id property. We then pass parameters into the route. Our first line creates a dictionary we will use to build our model, so we call it model. We instantiate our SelectedCategory using the parameters.id and a list of the CategoryTasks. We then add the category object and task list to our model, using the strings "category" and "tasks" as they key within the dictionary. We then return the view, using category.cshtml and pass our model into it. Simple, right?

---
    **category.cshtml**
    <h2>Here are all the tasks in this category:</h2>

    @if (@Model["tasks"].Count == 0)
    {
      <p>No tasks have been added yet!</p>
    }
    else
    {
      <ol>
        @foreach (var task in @Model["tasks"])
        {
          <li>@task.GetDescription()</li>
        }
      </ol>
    }
    <p><a href="/tasks/new">Add a new task</a></p>
    <p><a href="/">Return to Main Page</a></p>
---

#### 31. Run dnx kestrel to test all our routes

#### 32. Create Update() for Category class via PATCH.
* more info: https://www.learnhowtoprogram.com/c/c-database-basics/using-patch-and-delete-with-nancy

- This will update the **attributes** of particular **Category**. We'll call this method in our _HomeModule.cs_

  32. a) Start with spec in **CategoryTest.cs**
---
    [Fact]
    public void Test_Update_UpdatesCategoryInDatabase()
    {
    //Arrange
    string name = "Home stuff";
    Category testCategory = new Category(name);
    testCategory.Save();
    string newName = "Work stuff";

    //Act
    testCategory.Update(newName);


    string result = testCategory.GetName();

    //Assert
    Assert.Equal(newName, result);
    }
---

  32. b) Add Update(string ..) in Category.cs



## Information about CRUD operation in database w/Nancy
*https://www.learnhowtoprogram.com/c/c-database-basics/http-crud-and-*
- Creating information, reading, updating, & destroy it. (in NANCY ...HomeModule.cs)
- **POST method** CREATEs something on the server.
- **GET method** retrieves information w/o changing anything on the server.
- **PATCH & DELETE** common HTTP methods, to *update & destroy* information
 * Most browser can only make GET & POST requests.
 * To make a PATCH & DELETE requests
   - make a POST request & then include some extra information telling the server to treat it like a PATCH or DELETE.
   - example: when you edit the caption on a photo, you are telling your browser to make a PATCH request to the FB server, b/c you're updating the existing photo.
   - when you delete that old photo of you, you're telling your browser to make a DELETE request (removing that resources from the FB server).
   - In modern web applications, we use the same URL & different HTTP method to convey what we're trying to do.
 * example:
---
    Create a vehicle: POST http://localhost:5004/vehicles
    View a vehicle: GET http://localhost:5004/vehicles/5 (where 5 is the ID)
    Update a vehicle: PATCH http://localhost:5004/vehicles/5
    Delete a vehicle: DELETE http://localhost:5004/vehicles/5
---
   - Using the same URL and different HTTP methods to convey what action the server should take is part of a widely-accepted approach for designing web applications called **REST, or REpresentational State Transfer.**

   - if you follow REST each part of your application is modeled as a **resource.**

   - For now, you can think of each object in your application as a resource (_this isn't always true; for more complex applications_)

   - For **Viewing multiple vehicles**, the request would typically be **GET http://localhost:5004/vehicles**

   -For **Searching a vehicle/ Viewing a subset of all the vehicles**, the request might be **GET http://localhost:8000/vehicles?color=red&make=toyota.**
