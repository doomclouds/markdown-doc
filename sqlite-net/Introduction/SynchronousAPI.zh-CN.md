﻿﻿  # 介绍

  本库支持对SQLite的同步与异步操作。本指南将会演示如何同步的去调用您的数据库。 首先， 所有的同步API都必须使用`SQLiteConnection`类。 下面将展示如何初始化这个类。

  ```c#
  SQLiteConnection _connection;
  
  public SQLiteConnection InitialiseConnection() 
  {
      var DataSource = "DatabaseName.sqlite3";
  
      SQLiteConnectionString options = new SQLiteConnectionString(DataSource, false);
  
     _connection = new SQLiteConnection(options);
  }
  ```

  现在您已经初始化并打开了该对象的连接，现在可以开始查询数据库了。

  ## Select查询

  本库支持在数据库中执行Select查询的多种方式，您可以选择使用SQL或者LINQ的方式。两种方式各有千秋， 但是，使用Linq比SQL更加简洁，因为它使得您的代码更符合.Net的习惯 。下面是一个使用SQL方法的Select查询示例。 包括本节使用的类表映射

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

  `Query`方法需要一个泛型指示要查询的表格。在本例中，`Record`类被映射到`records`表。通过将`Record`指定为`Query`的泛型，现在可以使用实体映射关系来操作数据库表。

  下面是使用LINQ来执行同样的查询语句。

  ```c#
  public void GetRecords() 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
          
      var results = conn.Table<Record>().ToList();
  
      conn.Close();
  }
  ```

  正如你看到的那样，LINQ方式的查询比SQL方式要简介很多。您还可以使用其他LINQ条件(如Where或OrderBy)来补充这个“SELECT”查询，以优化结果。

  ```c#
  var results = conn.Table<Record>().Where(t => t.Age > 40).OrderByDescending(t => t.Age).ToList();
  ```

  ## Insert语句

  插入语句比Select查询更简单。 同样，这个库同时支持SQL和LINQ方式来执行语句。 下面是这两种方式的示例。

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

  LINQ版本：

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

  既然可以执行Insert语句，那么也可以执行Update语句。注意，映射表类**必须**包含一个主键，否则操作将不会生效。

  ## Update语句

  这是一个如何使用LINQ执行更新语句的快速演示。`conn.Update(record)` 方法使用`record`(在这里是`record.Id`)的主键在数据库中定位要更新的记录。注意主键是不能更新的。

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

  如果你不喜欢使用LINQ，这些查询也可以使用传统的SQL和` Query<T> `方法。

  ## Delete语句

  `Delete` 语句的工作原理与get请求类似。选择希望使用的数据库映射，然后传入查询参数。一般情况下，使用`record`的主键 。这确保主键关联的记录会被删除

  ```c#
  public void DeleteRecord(string id) 
  {
      var options = new SQLiteConnectionString(DataSource, false);
      var conn= new SQLiteConnection(options);
              
      conn.Delete<Record>(id);
      conn.Close();
  }
  ```
