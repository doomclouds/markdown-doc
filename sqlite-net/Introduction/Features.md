﻿# Features

  Documentation of the various features provided by the library

## ORM

  The ORM is able to take a .NET class definition and convert it to a SQL table definition. (Most ORMs go in the other direction.) It does this by examining all public properties of your classes and is assisted by attributes that you can use to specify column details.

  The following **attributes** are used:

  - `PrimaryKey` This property is *the* primary key of the table. Only single-column primary keys are supported.
  - `AutoIncrement` This property is automatically generated by the database upon insert. The propertytype should be an integer and should also be marked with the *`PrimaryKey`* attribute.
  - `Indexed` This property should have an index created for it.
  - `MaxLength` If this property is a `String` then *`MaxLength`* is used to specify the `varchar` max size. The default max length is 140.
  - `Ignore` This property will not be in the table.

  The following **data types** are supported in properties:

  - `Integers` are stored using the `integer` or `bigint` SQLite types.
  - `Boolean` are stored as `integers` with the value `1` designated as `true` while all other values are `false`.
  - `Enums` are stored as `integers` using the enum's value.
  - `Singles` and `Doubles` Floats are stored as `float` columns.
  - `Strings` are stored using `varchars` with a maximum size specified by the *`MaxLength`* attribute. If the attribute isn't specified, the max length defaults to 140.
  - `DateTimes` are stored as `datetime` columns and are subject to the precision offered by SQLite.
  - Byte arrays `byte[]` are stored as `blob` columns.
  - `Guid` structures are stored as `varchar(36)` columns.
