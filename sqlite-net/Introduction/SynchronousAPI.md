# Introduction

  The library supports both synchronous and asynchronous requests to your SQLite tables. This guide will demonstrate how to make synchronous calls to your database. First of all, all connections made using the synchronous API must be made using the `SQLiteConnection` class. The example below shows how to initialise this class

  ```c#
  SQLiteConnection _connection;
  
  public SQLiteConnection InitialiseConnection() 
  {
      var DataSource = "DatabaseName.sqlite3";
  
      SQLiteConnectionString options = new SQLiteConnectionString(DataSource, false);
  
     _connection = new SQLiteConnection(options);
  }
  ```

  Now your object has been initialised and a connection opened, you can now start making queries.

  ## Select Query

  The library supports multiple ways of performing a select query in your database and you can perform queries using SQL or LINQ. There is no real benefit of using one over the other, however, it is cleaner to use Linq over SQL as it makes your overall code base more consistent. Below is an example of a Select query using the SQL method. Included is the class table mapping used for this section

  ```c#
  [Table("records")]
  public class Record
  {
      [PrimaryKey] 
      [Column("id")]
      public Guid Id
      { get; set; }
  
      [Column("name")]
      public string Name
      { get; set; }
  
      [Column("age")]
      public int Age
      { get; set; }
  
      [Column("date")]
      public DateTime Date
      { get; set; }
  }
  
  public void GetRecords() 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
      
      string query = $"SELECT * FROM records";
      
      var results = conn.Query<Record>(query);
  
      conn.Close();
  }
  ```

  The `Query` method requires a type () in order to query the correct table. In this example, the `Record` class has been mapped to the `records` table. So by declaring `Record` as the `Query` type the library can now use the mapped table.

  Below you will find the exact same query performed using LINQ.

  ```c#
  public void GetRecords() 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
          
      var results = conn.Table<Record>().ToList();
  
      conn.Close();
  }
  ```

  As you can see LINQ is a much more condensed when compared to the SQL version of the query. You can supplement this `SELECT` query with additional LINQ conditions such as Where or OrderBy to refine your result.

  ```c#
  var results = conn.Table<Record>().Where(t => t.Age > 40).OrderByDescending(t => t.Age).ToList();
  ```

  ## Insert Statement

  Insert statements are even simpler than select queries. Again, this library supports both SQL and LINQ methods to execute your statement. An example of both methods can be seen below.

  ```c#
  public void InsertRecord() 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
          
      string query = $"INSERT INTO records VALUES ('{new Guid()}', 'Mark', '23', '{DateTime.Now}')";
      
      var results = conn.Query<Record>(query);
  
      conn.Close();
  }
  ```

  And the LINQ version:

  ```c#
  public void InsertRecord() 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
          
      var record = new Record { Id = new Guid(), Name = "Mark", Age = 23, Date = DateTime.Now };
      
      var results = conn.Insert(record);
  
      conn.Close();
  }
  ```

  Now that you can do insert statements you can perform Update too (conn.Update(record). keep in mind that in order to perform these operations your mapped table class **MUST ** include a mapped primary key, else the action will not work.

  ## Update Statement

  This is a quick demonstration of how to perform an update query using LINQ. The `conn.Update(record)` function uses the primary key of `record` (in this case, `record.Id`) to locate the record to update in the database. A consequence of this is that you cannot update the primary key ([you shouldn't be doing this anyway](https://stackoverflow.com/a/19316940/7511598)).

  ```c#
  public void UpdateRecord(Guid id) 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
          
      var record = new Record { Id = id, Name = "Mark", Age = 23, Date = DateTime.Now };
      
      var results = conn.Update(record);
  
      conn.Close();
  }
  ```

  If you prefer not to use LINQ, these queries can also be made using traditional SQL and the `Query<T>` method.

  ## Delete Statement

  The `Delete` Statement works similarly to a get request. You choose the table mapping you wish to use and then pass in a query parameter. In common instances, you delete records using the record's `Primary Key`. This property was set in the Record class.This ensures that the record with the associated PK will be removed.

  ```c#
  public void DeleteRecord(string id) 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
              
      conn.Delete<Record>(id);
      conn.Close();
  }
  ```

