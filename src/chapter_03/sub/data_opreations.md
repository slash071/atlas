#### Creating A Table With The CREATE TABLE Statement

You saw SQLite `CREATE TABLE` statement earlier. Let's take a closer look at it now. We'll start with the data types that you can use when declaring a table column: `INTEGER`, `REAL`, and `TEXT` will probably cover most of the use cases you have. You could use the `BLOB` type if you have to store something like an _image_. You wouldn't declare a column of type `NULL` but you might store a `NULL` value into a column that's declared as any of the other data types.

SQLite does not enforce strict typing by default. That means that you could insert a **numeric value** into a **text column** and vice versa. What this means is that something like this is completely legal (but at the same time, completely dubious):

```sql
CREATE TABLE TypeTest (Name TEXT, Id INTEGER);
INSERT INTO TypeTest VALUES (100, 'hello');

-- See the results yourself
SELECT * FROM TypeTest;
```

Fortunately, you can enforce strict typing by putting the keywords `STRICT` at the end of the statement:

```sql
CREATE TABLE TypeTestStrict (Name TEXT, Id INTEGER) STRICT;
-- Running command below will result and error!
INSERT INTO TypeTestStrict VALUES (100, 'hello');
```

This won't let you insert values that would violate type safety.

> After you create a table, you can look at its definition with the `.SCHEMA` command.

If you try to create a table that already exists you'll get an error. You can add `IF NOT EXISTS` to avoid getting this error:

```sql
CREATE TABLE IF NOT EXISTS TypeTest (Name TEXT, Id INTEGER, City TEXT);
```

But it will not create the table if it exists, even if you've changed the table's definition. If you want to make a change to a table's definition, you can either drop the table (losing all the data in the process) or use the `ALTER TABLE` command. We will discuss those options later.

We can also create a table that's the result of a query with create table name, as, select statement:

```sql
CREATE TABLE NewTypeTest AS
SELECT Name,
       Id,
       NULL as City
From TypeTest;
```

If we don't need a table anymore, we can drop it, but be careful because this is _permanent_:

```sql
DROP TABLE TypeTest;
DROP TABLE NewTypeTest;
```

so use this command with caution.

#### Setting Rules On Your Data

Your data can get messy quickly if there are no rules to govern what can be added to a table. Let's look at [some ways](https://www.sqlite.org/lang_createtable.html#the_primary_key) that you can avoid this type of mess. When you create a table, you can declare a number of constraints, which allow you to restrict what kinds of data can be inserted:

- PRIMARY KEY constraints
- UNIQUE constraints
- CHECK constraints
- NOT NULL constraints

Let's start with **UNIQUE constraints** first.

The UNIQUE constraint enforces a rule that a given value may appear **only once in the column** that it is applied to:

```sql
CREATE TABLE UniqueTest(Name TEXT, Id INTEGER UNIQUE);
```

If you try to insert a row containing a duplicate value for that column, it will raise an error:

```sql
INSERT INTO UniqueTest VALUES ('Jack', 100);
-- Command below will raise and error!
INSERT INTO UniqueTest VALUES ('Mauve', 100);
```

However, you can insert any number of nulls into a unique column. This is because nulls are a special case in SQL:

```sql
INSERT INTO UniqueTest VALUES ('Dave', NULL);
INSERT INTO UniqueTest VALUES ('Ted', NULL);
```

This is because two nulls are not even considered equal. Comparing them will return `NULL` (which is a blank). So you can use the `IS NULL` operator to check for sure whether the result is null:

```sql
SELECT NULL = NULL IS NULL;
```

That's going to return `1` here, which is SQLite's way of saying that `NULL` equals `NULL` resolves to `NULL`. `1` is SQLite's way of saying `TRUE`.

The `NOT NULL` constraint prohibits null values.

#### Adding Data To A Table

You saw the `INSERT` statement earlier which lets you insert individual rows as well as multiple rows into a table. Let's set up some sample data and take a look at some of the other ways you can get data into a table.

```sql
CREATE TABLE Item
(Name TEXT NOT NULL,
 Id INTEGER PRIMARY KEY);

INSERT INTO ITEM
VALUES
('Iron', NULL),
('Leather', NULL);
```

You can insert into a table using the results of a `SELECT` statement:

```sql
CREATE TABLE Item2
(Name TEXT NOT NULL,
Id INTEGER PRIMARY KEY);

INSERT INTO Item2
(Name, Id)
SELECT * FROM Item;
```

However, the columns in the `SELECT` statement must match the columns in the `INSERT` statement in order for this to work. If `SELECT` pulls the columns in a different order or if there are extra columns, the database won't know how to align the data correctly, and it will cause an error.

You can omit columns but you must include any columns that have a `NOT NULL` constraint. Otherwise, SQLite will try and fail to insert a null. Let's try that here and see what happens:

```sql
CREATE TABLE Item3
(Name TEXT NOT NULL,
Id INTEGER PRIMARY KEY,
Weight INTEGER NOT NULL);

-- This will raise and error! Weight can't be empty
INSERT INTO Item3
(Name, Id)
SELECT * FROM Item;
```

We just skiped the `Weight` column and that violates our constraint.

> Just ensure that whenever you're inserting data into a table with `NOT NULL` constraints, you:
>
> - Include values for all `NOT NULL` columns in your `INSERT` statement.
> - Use default values if applicable.
> - Match the order and count of columns correctly in your `INSERT` and `SELECT` statements.

You can also import from a **CSV** file. SQLite will let you create a new table from a CSV file:

```sql
.mode csv
.import <filename>.csv <table>
.header on
SELECT * FROM <table>;
```

You can also import into an existing table but if you do that you'll need to skip the header if there is one. Otherwise, you would be importing the header as a row:

```sql
.mode csv
.header on

CREATE TABLE QuestTable
(Quest TEXT UNIQUE,
XP INTEGER);

.import --skip 1 quest.csv QuestTable
```

You can do that with the `--` skip option with the dot import command.

---

**UPSERT** is a combination of `UPDATE` and `INSERT`. It enables you to either insert a new row into a table or update an existing row if there’s a conflict, such as a violation of a uniqueness constraint (like primary keys or unique indices).

When you want to add new data but also need to ensure that existing data isn’t duplicated, **UPSERT** simplifies your SQL operations. Instead of writing separate INSERT and UPDATE statements, you can accomplish both in a single command.

In example below, we're defining the inventory table such that each player item pair must be unique:

```sql
CREATE TABLE Inventory
(PlayerId INTEGER,
ItemId INTEGER,
Quantity Integer,
PRIMARY KEY (PlayerId, ItemId)
);

INSERT INTO Inventory VALUES (1, 1, 5);
```

Inserting a row for the same player and item would violate this rule. However, using the `ON CONFLICT` clause gives us an out:

```sql
INSERT INTO Inventory VALUES (1, 1, 2)
ON CONFLICT (PlayerId, ItemId) DO
UPDATE SET Quantity = Quantity + excluded.Quantity;
```

When there's a conflict on `PlayerId` and `ItemId`, the `UPDATE` statement sets the `Quantity` in the table to the existing `Quantity` (which is 5) plus the `Quantity` from the new row that was excluded (which is 2). So the final value becomes **5 + 2 = 7**.

We can refer to columns from the row that produced the conflict with the `excluded` prefix, so in this case to grab the `Quantity` from the row that was rejected, we say `excluded.Quantity`.

#### Modifying An Existing Table

You will sometimes want to modify data that already exists in a table. You saw how a failed `INSERT` can turn into an `UPDATE`, but let's look at the `UPDATE` statement in-depth, now.

First, let's make a temporary copy of the table from the database:

```sql
CREATE TEMPORARY TABLE Invoice2
AS SELECT * FROM Invoice;
```

We use the `TEMPORARY` keyword to create a table that will be discarded when we quit the command-line client. You can use the `UPDATE` command to update a single row or a bunch of rows at once. It all depends on what you put in the `WHERE` clause. Let's change all of the billing countries that are **USA** to **US**:

```sql
UPDATE Invoice2
SET BillingCountry = 'US'
WHERE BillingCountry = 'USA';
```

With that done, Let's also to update Apple's record to use their most current address, and take a look and see the results of making those changes:

```sql
UPDATE Invoice2
SET BillingAddress = 'One Apple Park Way'
WHERE CustomerId = 19;

SELECT *
FROM Invoice2
WHERE CustomerId = 19;
```

> Whether you update a single row, or many rows, depends entirely what you put into the `WHERE` clause, so be careful. If you skip the `WHERE` clause, you should have a good reason for doing so.

Let's try something out. This command could be a total disaster because we're not specifying a `WHERE` clause:

```sql
UPDATE Genre SET GenreId = 100;
```

This would set **GenreId** to a hundred for everything. Fortunately, there's a primary key index on **GenreID** that kept us out of trouble. You can't always count on an index to save you, though.

You can also use DB Browser to edit a table using a spreadsheet-like interface. Open the database, then find the table you want. Right click and choose _Browse Table_. Look for the value you want to change, edit it and then when you're done, you can click _Write Changes_ to write the changes back to the database and make them permanent.

You can also change the structure of a table itself. Use the ALTER TABLE command to add, rename, or even delete columns:

```sql
ALTER TABLE Invoice2 RENAME Total TO InvoiceTotal;
```

If you delete a column that is involved with certain constraints, such as a foreign key, you will get an error:

```sql
ALTER TABLE Invoice2 DROP COLUMN GenreId;
-- This will result:
-- Error: in prepare, cannot drop PRIMARY KEY column: "GenreId" (1)
```

You are limited in terms of what constraints you can put on columns that you add to a table. Let's set up some sample data so we can test this out:

```sql
CREATE TABLE Player
(Name TEXT);

-- Add Some value
INSERT INTO Player VALUES ('Eric');

-- This does not going to work!
ALTER TABLE PLAYER ADD Id PRIMARY KEY;
-- Error: in prepare, Cannot add a PRIMARY KEY column (1)
```

Okay, now here's one such limitation. You can't use the `ALTER TABLE` command to add a primary key, or a column that has a unique constraint.

Also, if you add a column with a `NOT NULL` constraint to a table with existing rows, you will need to supply a default value. Let's try it now:

```sql
ALTER TABLE PLAYER ADD HitPoints NOT NULL;
```

and that gives us an error. So let's add a default value:

```sql
ALTER TABLE Player ADD HitPoints NOT NULL DEFAULT 20;
```

There. Now, what that will do is set all of those to that default value, if they exist.

You can also create a new table with a fresh definition and copy data from the old table into it. Here's how you could get around the restriction against adding a primary key to an existing table:

- Renamed the table
- Created a new table with the old name and the definition that we want
- Use the INSERT statement to insert values from the _renamed old table_ and put them into the _new table_

Let's try it out:

```sql
ALTER TABLE Player RENAME TO OldPlayer;

CREATE TABLE Player
(Name TEXT,
HitPoints Integer,
Id INTEGER PRIMARY KEY);

INSERT INTO Player (Name, Id)
SELECT Name, HitPoints FROM OldPlayer;

DROP TABLE OldPlayer;
```

SQLite automatically assigns the primary key when it is null or unspecified, so that will take care of assigning those values. If you're using DB Browser, you can accomplish the same thing without quite as many steps.

First, We will create a new in-memory database. Create the player table and just give it one column **Name** of Type `TEXT` and then we'll insert a row. Now we've got one existing row. We can now go back to the database structure right click and choose _Modify The Table_, And it will allow us to change the structure.

#### Deleting Data

The time will inevitably come when you want to delete some data from a database table. In some ways, the semantics of deletion are similar to updating.

You can use the delete statement to delete a single row:

```sql
DELETE FROM Invoice3
WHERE InvoiceId = 35;
```

In this case, we're just deleting the one where `InvoiceID` equals `35` but you can also delete a bunch of rows at once:

```sql
DELETE FROM Invoice3
WHERE BillingCountry = 'USA';
```

This one deletes everything where the `BillingCountry` is `USA`.

> Whether you update a single row or many rows depends entirely on what you put in the `WHERE` clause, so be careful. If you skip the `WHERE` clause, you're going to delete everything in the table.

This command would be a disaster if we were working on data that we really needed:

```sql
DELETE FROM Invoice3;
```

This command will delete all the data in the specified table, but the table structure remains intact. Be cautious, as this action cannot be undone!

Now that you understand how to create, add, modify and delete data, let's look at some advanced methods for querying it.
