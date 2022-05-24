﻿﻿﻿## 异步API

  你可以在不需要阻塞的地方使用异步API。 为了提高响应速度，你可能在移动应用程序使用异步API。

  异步库使用任务并行库(TPL)。因此，可以正常使用`Task`对象以及`async`和`await`关键字。

  一旦你定义了你的实体，你可以通过调用`CreateTableAsync`来自动生成表：

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  conn.CreateTableAsync<Stock>().ContinueWith((results) =>
  {
      Debug.WriteLine("Table created!");
  });
  ```

  可以使用insert在数据库中插入行。如果表包含一个自动递增的主键，那么该键的值将在插入后返还给你：

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

  `UpdateAsync`和`DeleteAsync`用法很相似。

  查询数据最直接的方法是使用`Table`方法。 它将返回一个`AsyncTableQuery`实例，然后你可以通过`WHERE`子句和/或添加`ORDER BY`来添加约束查询条件。 知道调用`ToListAsync`、`FirstAsync`或者`FirstOrDefaultAsync`才会执行真正的数据库查询语句。

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  var query = conn.Table<Stock>().Where(v => v.Symbol.StartsWith("A"));
  
  query.ToListAsync().ContinueWith((t) =>
  {
      foreach (var stock in t.Result)
          Debug.WriteLine("Stock: " + stock.Symbol);
  });
  ```

  下面是一些低级API。 你也可以通过`QueryAsync`方法直接查询数据库。除了`InsertAsync`等提供的更改操作之外，你还可以使用`ExecuteAsync`方法来直接更改数据库中的数据。

  另一个有用的方法是`ExecuteScalarAsync`。 你可以从数据库轻松返回一个标量值：

  ```c#
  var conn = new SQLiteAsyncConnection("foofoo");
  conn.ExecuteScalarAsync<int>("select count(*) from Stock", null).ContinueWith((t) =>
  {
      Debug.WriteLine(string.Format("Found '{0}' stock items.", t.Result));
  });
  ```
