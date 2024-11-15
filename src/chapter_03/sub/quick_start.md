## Quick Start

If we start SQLite without specifying a database, there wont be any tables to interact with. So by running `sqlite3` command we see this output:

    Connected to a transient in-memory database.

However, we can create a database with sqlite by specifying a file name when we are running the `sqlite3` command:

```bash
sqlite3 <filename>.db
```

Doing so we can start creating tables and adding data to them:

```sql
-- Create a new table with specified columns and their data types.
CREATE TABLE <table> (<column> STRING, <column> INT);

-- Insert a new row into the specified table with given values
INSERT INTO <table> VALUES ('Eric', 100);

.tables -- See available tables
.help   -- More info at https://sqlite.org/cli.html
.exit   -- Exit Program

-- All records from '<table>' where the column starts with 'E'
SELECT * FROM <table> WHERE <column> LIKE 'E%';

 -- Update the <column> in <table>, changing the value from 'Science' to 'Sci-Fi'
UPDATE <table> SET <column> = 'Sci-Fi' WHERE <column> = 'Science';
```

SQLite command-line shell has a few key notes:

- Quick and easy way to interact with the SQLite database
- Supports all SQLite commands and syntax
- Allows you to create a database or work with an existing one

If a GUI front end is more your thing try using `sqlitebrowser` app also known as `dbbrowser` which is free and open source. Other options include `SQLiteStudio` and `vscode-sqlite`.

#### Retrieving Data with SELECT

`SELECT` statement lets you retrieve and display rows from a database table. In it's simplest form, we can type command below to retrieve all the rows and columns from the table you specified:

```sql
SELECT * FROM <table>;
```

That can sometimes be a lot of data. Let's refine the `SELECT` statement abit:

```sql
-- Selects the first 10 rows from the specified table
SELECT * FROM <table> LIMIT 10;

-- Selects the specified column from the table
SELECT <column> FROM <table>;
```

If you want to do more than just look at the results of a query, you can save the results to a file such as `CSV` or `Comma Separated value` that could then open in Excel or another program:

```sql
.mode csv                -- Set the output mode to CSV format
.header on               -- Enable headers in the output (column names will be included)
.output <filename>.csv   -- Redirect the output to a specified CSV file
SELECT * FROM <table>;   -- Execute a SQL query to select all data from the specified table

.output stdout           -- Reset output to standard output
.mode list               -- Change output mode to list format
-- .header off            -- header can be on optionally

.shell open <filename>.csv    -- Execute a shell command to open the specified CSV file
```

Exporting process can be done in `sqlitebrowser` too.

#### Filtering your SELECT with the WHERE clause

In its most basic form the `WHERE clause` lets you specify a filter condition such as:

```sql
-- Selects all columns from the table where the value of 'Total' is greater than 10.
SELECT * FROM <table> WHERE Total > 10;
```

You can combine conditions to narrow things down even more, such as this statement:

```sql
-- This query retrieves all columns from the Invoice table where the Total is
-- greater than 10 and the InvoiceDate falls within the first quarter of 2024.
SELECT * FROM Invoice WHERE Total > 10 AND InvoiceDate BETWEEN '2024-01-01' AND '2024-03-31';
```

`AND` connects conditions that all need to be satisfied. We can also use `OR` condition:

```sql
-- This query selects all columns from the Invoice table where the Total is
-- greater than 10 and the InvoiceDate falls within specified date ranges.
SELECT *
FROM Invoice
WHERE Total > 10
AND (InvoiceDate BETWEEN '2023-01-01' AND '2023-03-01'
OR InvoiceDate BETWEEN '2024-01-01' AND '2024-03-31');
```

The `OR` clause is put in parentheses to group the logical expressions. If you don't add parentheses in your SQL query, the logical operators would follow their default precedence, where `AND` is evaluated before `OR`. This means the condition could be interpreted differently, likely leading to unintended results. Specifically, without parentheses, the query might first check if `Total > 10` and then combine that result with one of the `InvoiceDate` conditions, potentially excluding some valid records that you intended to include. Parentheses ensure the correct grouping of conditions, thus leading to the desired filtering of results.

We can also use any of the SQLite many functions in a `SELECT` statement. For example we can use `LOWER` function to implement a kind of case insensitive search:

```sql
-- Retrieves all rows from the Genre table where the value in the Name column
-- is exactly 'rock' (case-insensitively)
SELECT * FROM Genre WHERE LOWER(Name) = 'rock';
```

We can use options with oprators as well:

```sql
-- Retrieves all rows from the Genre table where the Name column
-- contains 'rock' (case-insensitively)
SELECT * FROM Genre WHERE LOWER(Name) LIKE '%rock%';
```

#### Grouping Your Results

When working with SQLite you will often want to look at your query results in aggregate. The simplest aggregation is one where you use an aggregate function without any kind of grouping:

```sql
.header on
SELECT SUM(Total) AS OverallTotal FROM <table>;
```

This will aggregate everything in the table into a single result. The `AS OverallTotal` part of the query tells SQLite what name to use for the output of `SUM(Total)`.

IF you supply a group by clause you get more than one row in your query results (Or you can get more than one row in your query results). You also need to specify the grouping columns in the statement list of columns in the select statements list of columns:

```sql
SELECT InvoiceDate SUM(Total) AS DailyTotal
FROM Invoice
GROUP BY InvoiceDate;
```

When we run this its will give us sums of the `InvoiceTotal` by day.

> Notice how `InvoiceDate` needs to appear in both the list of columns and in the group by clause.

The results won't be easy to read if they're coming at you in an arbitrary order. So we can also add an order by clause to specify a sort order:

```sql
SELECT InvoiceDate, SUM(Total) AS DailyTotal
FROM Invoice
GROUP BY InvoiceDate
ORDER BY InvoiceDate;
```

If we want to see the results ordered by largest to smallest `Total` we can use the `ORDER BY` clause with the `DEsC` keyword to sort that column in descending order:

```sql
SELECT InvoiceDate, SUM(Total) AS DailyTotal
FROM Invoice
GROUP BY InvoiceDate
ORDER BY SUM(Total) DESC;
```

You can't refer to aggregate functions within the `WHERE` clause because the `WHERE` clause affects which rows are selected before the aggregate functions computed. But the `SELECT` statement offers `HAVING` clause that allows you to refer to the aggregate values after they are calculated:

```sql
SELECT InvoiceDate, SUM(Total) AS DailyTotal
FROM Invoice
GROUP BY InvoiceDate
HAVING SUM(Total) > 10
ORDER BY SUM(Total) DESC;
```

You can also group by more than one column and you can perform more than one aggregation. For example:

```sql
SELECT InvoiceDate,
       CustomerId,
       SUM(Total) AS DailyTotal,
       COUNT(CustomerId) AS InvoiceCount
FROM Invoice
GROUP by InvoiceDate, CustomerId;
```

SQLite include a number of of aggregate functions you could use and they're documented at [this page](https://www.sqlite.org/lang_aggfunc.html).
