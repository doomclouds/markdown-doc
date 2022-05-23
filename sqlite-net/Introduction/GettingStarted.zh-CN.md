﻿﻿﻿# 开始

设计 **sqlite-net** 是为了在.NET环境下可以轻松的使用SQLite数据库。

  本库包含创建数据表格的一些简单特性。在下面的基础示例当中，您将学习如何基于对象映射快速生成数据库表，以及如何从数据库中插入和查询数据。

  当您第一次启动一个项目时，您可能已经创建了一个带有多个表的SQLite数据库。 虽然这种方法没有什么问题，但是本库提供了一种基于对象关系映射(ORM Object-Relational Mappings)的方式自动根据您设计的实体模型来创建数据库表格。

 让我们创建两个类，分别是 `Stock` 和 `Valuation`. 这两个类充当我们如何使用ORM (Object-Relational Mappings)的示例类。

  ```c#
  [Table("Stocks")]	 
  public class Stock		
  {		
      [PrimaryKey, AutoIncrement]
      [Column("id")]		
      public int Id { get; set; }	
  
      [Column("symbol")]			
      public string Symbol { get; set; }		
  }		
      		
  [Table("Valuation")]
  public class Valuation		
  {		
      [PrimaryKey, AutoIncrement]	
      [Column("id")]			
      public int Id { get; set; }	
  	
      [Indexed]		
      [Column("stock_id")]		
      public int StockId { get; set; }
  
      [Column("time")]				
      public DateTime Time { get; set; }	
  
      [Column("price")]			
      public decimal Price { get; set; }		
  }		
  ```

  类和每个字段上面的特性帮助库更有效地映射数据库关系:您不需要在数据库本身中创建或指定实体关系。

  下一步，创建 `DatabaseHandler` 类。这个类将处理表的生成、查询和命令。

  ```c#
  public class DatabaseHandler {
  
      private SQLiteConnection _db;
      
      public DatabaseHandler() {
          
          _db = new SQLiteConnection("path/to/your/database");
          _db.CreateTable<Stock>();
          _db.CreateTable<Valuation>();		
      }
  }	
  ```

  在构造器中, 我们创建了 `SQLiteConnection` 对象并将SQLite数据库的路径传递到括号中。然后我们创建`Stock`表和`Valuation`表。这就是ORM发挥作用的地方。如果我们运行这个应用程序，这些表将自动映射到SQLite数据库，我们将能够使用SQL查询访问它们。 如果SQLite数据库中不存在这些表，则会自动生成它们。如果它们确实存在，表中的任何现有数据都将被迁移，并且表将被更新(如果添加了新字段)。

  既然已经成功映射了数据库，我们就可以开始对数据执行查询了。让我们从向`Stock`表插入一行数据开始。

  ```c#
  public void AddStock() {	
      
      var stock = new Stock{		
          Symbol = "MSFT"		
      };
  
      _db.Insert( stock );		
  }
  ```

  就像这样，我们在`Stock`表中插入了一行数据。 主键是自动生成的，所以我们不需要指定它:我们所需要做的就是创建一个新的`Stock `对象，并将其传递给`_db.Insert`。

 如果我们查询数据库，我们将能够看到新记录。

  ```c#
  public void GetStocks()		
  {
      var stocks = _db.Query<Stock>("SELECT * FROM Stocks");
      
      foreach(var stock in stocks)
      {
          Console.WriteLine(stock.Symbol);
      }	
  }		
  ```

  现在您已经学会基本库使用，下面我们了解下 [同步API](SynchronousAPI.zh-CN.md).
