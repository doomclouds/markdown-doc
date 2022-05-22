﻿# Automatic Migrations 

   Code changes. Sometimes you need to associate new values with tables (entities) after the database has already been deployed and used for some time.

   Thanks to the relational model, entities in the database can have values associated with them simply by adding new tables and relating those new tables to the entity.

   This, however, is not always ideal since it requires performing joins whenever you need to query those values for the entity.

   To help, sqlite-net supports automatic migrations.

   ## Details

   Tables are automatically migrated when `CreateTable` is called. If the table already exists, its metadata is queried and compared to the structure of the class passed to `CreateTable`. If the two structures are different, then the table is automatically migrated to match the code.

   The automatic migration, currently, only supports *adding new columns*. If your classes have new properties that are not associated with columns in the table, then `alter table ... add column` commands will be executed to bring the database up to date. These new columns will not have default values and will therefore be `null`.

   Possible additional migrations *that are not supported* include, deletion of columns, changing the types of columns, and renaming columns. If there is demand, these migrations can be added.
