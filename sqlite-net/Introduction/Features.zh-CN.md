# 特性

  本库提供的各种特性的说明文档。

## ORM

  ORM可以让.NET类的定义转变为SQL语句的定义。它们通过类的公开属性及属性的特性来定义表格的列。

  支持的**特性**如下 ：

  - `PrimaryKey` 这个属性是表的主键。只支持单列主键。
  - `AutoIncrement` 此属性在插入时由数据库自动生成。属性类型应该是一个整数，该属性还应该用'`PrimaryKey ` 属性标记。
  - `Indexed` 为该属性创建一个索引。
  - `MaxLength`如果此属性为`String`，则`MaxLength`用于指定`varchar`的最大大小。默认最大长度为140。
  - `Ignore` 此属性将被数据库表忽略。

  属性支持的数据类型如下：

  - `Integers` 对应SQLite`integer` 或 `bigint` 。
  - `Boolean` 使用`integers`来表示，1是`true`，0是`false`。
  - `Enums` 使用`integers`来表示。
  - `Singles` 、`Doubles`和`Floats`对应 `float`。
  - `Strings` 使用 `varchars`表示，使用`MaxLength`来指定长度。如果未指定默认是140。
  - `DateTimes` 使用为`datetime`表示，并受SQLite提供的精度的影响。
  - `byte[]` 使用`blob`表示。
  - `Guid` 使用`varchar(36)`表示。
