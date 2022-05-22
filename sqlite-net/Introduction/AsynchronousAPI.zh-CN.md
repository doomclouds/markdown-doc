# Synchronous API

  You can use the "asynchronous API" where calls do not block. You may care to use the asynchronous API for mobile applications in order to increase responsiveness.

  The asynchronous library uses the Task Parallel Library (TPL). As such, normal use of Task objects, and the async and await keywords will work for you.

  Once you have defined your entity, you can automatically generate tables by calling CreateTableAsync:

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  conn.CreateTableAsync<Stock>().ContinueWith((results) =>
  {
      Debug.WriteLine("Table created!");
  });
  ```

  You can insert rows in the database using Insert. If the table contains an auto-incremented primary key, then the value for that key will be available to you after the insert:

  ```c#
  Stock stock = new Stock()
  {
      Symbol = "AAPL"
  };
  
  var conn = new SQLiteAsyncConnection("foofoo");
  conn.InsertAsync(stock).ContinueWith((t) =>
  {
      Debug.WriteLine("New customer ID: {0}", stock.Id);
  });
  ```

  Similar methods exist for UpdateAsync and DeleteAsync.

  Querying for data is most straightforwardly done using the Table method. This will return an AsyncTableQuery instance back, whereupon you can add predicates for constraining via WHERE clauses and/or adding ORDER BY. The database is not physically touched until one of the special retrieval methods - ToListAsync, FirstAsync, or FirstOrDefaultAsync - is called.

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  var query = conn.Table<Stock>().Where(v => v.Symbol.StartsWith("A"));
  
  query.ToListAsync().ContinueWith((t) =>
  {
      foreach (var stock in t.Result)
          Debug.WriteLine("Stock: " + stock.Symbol);
  });
  ```

  There are a number of low-level methods available. You can also query the database directly via the QueryAsync method. Over and above the change operations provided by InsertAsync etc you can issue ExecuteAsync methods to change sets of data directly within the database.

  Another helpful method is ExecuteScalarAsync. This allows you to return a scalar value from the database easily:

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  conn.ExecuteScalarAsync<int>("select count(*) from Stock", null).ContinueWith((t) =>
  {
      Debug.WriteLine(string.Format("Found '{0}' stock items.", t.Result));
  });
  ```
