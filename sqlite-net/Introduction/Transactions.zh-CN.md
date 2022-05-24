# 事务

   本库支持嵌套事务。它们在同步API和异步API中都可以使用。

## 同步API

   在使用同步的`SQLiteConnection`时，执行内部事务需要将`Action`的代理传递给`RunInTransaction(Action)`方法：

   ```c#
   var db = new SQLiteConnection(path);
   db.RunInTransaction(() => {
       // database calls inside the transaction
       db.Insert(stock);
       db.Insert(valuation);
   });
   ```

   注意在`Action`调用`SQLiteConnection`时使用的是相同实例(在上面的例子中是`db`对象)。

## 异步API

   在使用异步的`SQLiteAsyncConnection`时，执行内部事务需要将`Action<SQLiteConnection>`的代理传递给`RunInTransactionAsync(Action<SQLiteConnection>)`方法：

   ```c#
   var db = new SQLiteAsyncConnection(path);
   db.RunInTransactionAsync(tran => {
       // database calls inside the transaction
       tran.Insert(stock);
       tran.Insert(valuation);
   });
   ```

   注意，在`Action`委托中使用的是`SQLiteConnection`实例(在上面的例子中名为`tran`)，而不是调用的`SQLiteAsyncConnection`实例(在上面的例子中名为`db`)。

## 低级API

   大多数时候，`RunInTransaction `和`RunInTransactionAsync `方法可以满足需求。 如果出于某种原因你需要更多的控制，`SQLiteConnection`包含了一组处理事务的替代方法:

   - `BeginTransaction` 启动一个新事务，如果事务已经打开，则抛出异常。
   - `SaveTransactionPoint` 在事务中创建一个可以回滚的新保存节点。如果没有正在进行的事务，它会启动一个新的事务，因此它可以作为`BeginTransaction `的安全替代。
   - `Commit` 提交当前事务。
   - `Rollback` 完全回滚当前事务。
   - `RollbackTo` 可以回滚到`SaveTransactionPoint`返回的现有保存节点。

## 处理异常

   如果你的事务抛出一个异常，在事务回滚后将会被`RunInTransaction`的调用者所捕获。
