# Transactions

   Library supports nested transactions. They are available both in synchronous and asynchronous API.

   ## Synchronous API

   When using synchronous `SQLiteConnection` the database calls to be executed inside a transaction should be passed as an `Action` to `RunInTransaction(Action)`:

   ```c#
   var db = new SQLiteConnection(path);
   db.RunInTransaction(() => {
       // database calls inside the transaction
       db.Insert(stock);
       db.Insert(valuation);
   });
   ```

   Note that inside the `Action` delegate the same instance of `SQLiteConnection` is being used as `RunInTransaction` was called on (named `db` in the example above).

   ## Asynchronous API

   When using asynchronous `SQLiteAsyncConnection` the database calls to be executed inside a transaction should be passed as an `Action<SQLiteConnection>` to `RunInTransactionAsync(Action<SQLiteConnection>)`:

   ```c#
   var db = new SQLiteAsyncConnection(path);
   db.RunInTransactionAsync(tran => {
       // database calls inside the transaction
       tran.Insert(stock);
       tran.Insert(valuation);
   });
   ```

   Note that inside the `Action` delegate an instance of `SQLiteConnection` (named `tran` in the example above) is being used, not the instance of `SQLiteAsyncConnection` instance that `RunInTransactionAsync` was called on (named `db` in the example above).

   ## Low Level API

   Most of the time `RunInTransaction` and `RunInTransactionAsync` methods should be all you need. If for some reason you need more control, `SQLiteConnection` contains an alternative set of methods for dealing with transactions:

   - `BeginTransaction` starts a new transaction. It throws an exception if a transaction is already open
   - `SaveTransactionPoint` creates a new save point in the transaction to which a rollback is possible. If no transaction is in progress, it starts a new one, therefore it can be used as a safer alternative to `BeginTransaction`.
   - `Commit` commits the current transaction.
   - `Rollback` completely rolls back the current transaction.
   - `RollbackTo` can rollback to an existing save point, returned by `SaveTransactionPoint`.

   ## Handling Errors

   If your transaction code throws an exception, it will be bubbled up to the caller of `RunInTransaction` after the transaction is rolled back.

   [ Add a custom footer](https://github.com/praeclarum/sqlite-net/wiki/_new?wiki[name]=_Footer)
