# Getting Started 

 **sqlite-net** is designed to make working with SQLite very easy in a .NET environment.

  The library contains simple attributes that you can use to control the construction of tables. In this base example, you will learn how to quickly generate tables based on object mappings, as well as insert and select items from your database.

  When you first start a project, you may already have an SQLite database set up with multiple tables. There is nothing wrong with this approach - however, this library allows you to make use of ORMs (Object-Relational Mappings) to generate new tables based on your models.

  Let's create two new classes called `Stock` and `Valuation`. These classes will act as our first ORMs (Object-Relational Mappings).

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

  The decorators above the class and each field help the library map your databases more efficiently: you don't need to create or specify entity relationships in the database itself.

  Next, we create a `DatabaseHandler` class. This class will handle the generation of our tables, as well as our queries and commands.

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

  Inside of our constructor, we are creating a new instance of the `SQLiteConnection` class and pass the path of our SQLite database inside the brackets (use a mixture of strings and filesystem helpers like `Xamarin.Essentials`). Then we are creating the Stock and Valuation tables. This is where the ORM part comes in. If we run this application, the tables will be automatically mapped to the SQLite database and we will be able to access them using SQL queries. If the tables do not exist in the SQLite database, they will be generated. If they do exist, any existing data in the table will be "Migrated" and the tables will be updated (if new fields have been added).

  Now that the database has been successfully mapped, we can begin performing queries on the data. Let's start by inserting a new row into the `Stock` table.

  ```c#
  public void AddStock() {	
      
      var stock = new Stock{		
          Symbol = "MSFT"		
      };
  
      _db.Insert( stock );		
  }
  ```

  And just like that, we have inserted a new row into the `Stock` table. The Primary key is automatically generated, so we do not need to specify it: all we needed to do was create a new `Stock` object and pass it to `_db.Insert`.

  If we query the database, we will be able to see the new record.

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

  And that's it! Now that you're all set, learn about the basic commands using the [Synchronous API Docs](Synchronous API.md).
