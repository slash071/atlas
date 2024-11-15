#### Combining Multiple Tables With `JOIN`s

So far, most of the queries you've seen have retrieved data from just one table, and most of the time you'll be working with data from multiple tables, so it's important to join them properly to get the results you want. When you learned about _foreign keys_ earlier, you got a glimpse of the `JOIN` clause. This allows you to link multiple tables when you query them together. Here's an example that gets the count of **InvoiceId**s for every customer who has a company name associated with their record in the **Customer** table:

```sql
SELECT Company, COUNT(InvoiceId) FROM Customer
JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
WHERE Company IS NOT NULL
GROUP BY Company;
-- Selects the company name and the count of invoices for each company
-- Joins the Customer table with the Invoice table on the CustomerId column
-- Filters out any records where the company name is null
-- Groups the results by the company name
```

The `WHERE Company IS NOT NULL` clause takes care of only selecting customers with company names. You may be wondering what happens when you query two tables without a `JOIN` clause. In some cases this is useful, but in general, you'll get results that combine data from tables incorrectly. Here's what happens when we try the preceding query without the `JOIN` clause:

```sql
SELECT Company, COUNT(InvoiceId)
FROM Customer, Invoice
WHERE Company IS NOT NULL
GROUP BY Company;
```

Now, we might see huge inflation in the number of invoices associated with each customer:

- **What it does**: Combines every row from the Customer table with every row from the Invoice table. This means each customer will be paired with every single invoice.
- **Effect**: If you have, for example, 10 customers and 100 invoices, you end up with 1000 combinations (10 x 100). Each customer appears 100 times, artificially inflating the count of the invoices associated with each company.

> When you don't use a proper `JOIN` and instead rely on a comma-separated list of tables in the `FROM` clause, this creates what's called a **Cartesian product** (or cross join).

what happens if we have a company that has no invoices? Let's add one and see what happens to the correct query that we ran earlier:

```sql
INSERT INTO Customer
(CustomerId, FirstName, LastName, Company, Email)
VALUES (NULL, 'Brian', 'Lee', 'LinkedIn', 'sample@linkedin.com');

SELECT Company, COUNT(InvoiceId) FROM Customer
JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
WHERE Company IS NOT NULL
GROUP BY Company;
```

We don't need to specify a **CustomerId** because it's a primary key, so we will just supply `NULL` and that'll cause it to get populated.

As you can see, it's not there. This is because the `JOIN` clause is restrictive by default. It only _returns rows where the table on the left of the `JOIN` (customer) and the table on the right have matching values_ for the key **CustomerId**. That's the intersection of these two sets of **CustomerId** keys.

SQLite offers a `JOIN` called **LEFT OUTER JOIN**. The outer in the name suggests that the leftmost table receives special favor. Its rows are represented in the results, even if their **CustomerId** values fall outside of that intersection:

```sql
SELECT Company, COUNT(InvoiceId) FROM Customer
LEFT OUTER JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
WHERE Company IS NOT NULL
GROUP BY Company;
```

So in the case of a customer that has no matching invoice records, (such as LinkedIn), the _Invoice_ columns are represented as `NULL`s, and so the `COUNT(InvoiceId)` calculation may returns zero.

A **FULL OUTER JOIN** is similar to a LEFT OUTER JOIN, except that rows from both tables are represented in the results, even if they fall outside the intersection of the JOIN keys:

```sql
SELECT COALESCE(Company, 'Unknown'), COUNT(InvoiceId)
FROM (SELECT * FROM Customer WHERE Company IS NOT NULL) Customer
FULL OUTER JOIN Invoice ON Invoice.CustomerId = Customer.CustomerId
GROUP BY Company;
```

So what we do here is to contrive an example by using a subquery that filters the customer table to only those with a company name, and so we refer to that subquery in the `FROM` clause.

The invoices that are associated with customers who are unaffiliated are represented in that first row where company is _Unknown_. We use the `COALESCE` function to replace any `NULL` company names with _Unknown_. The nulls are a consequence of that FULL OUTER JOIN because nothing was found in that subquery that represent a diversion of the **Customer** table for which all rows have a company name.

#### Basic Functions

SQLite offers a rich set of scalar functions that you can use on column values. The word **scalar** indicates that they operate on a single value at a time, as opposed to aggregate functions such as `COUNT` and `SUM`, which operate on many rows at once. You'll want to check out the [SQLite documentation](https://sqlite.org/lang_corefunc.html) to see a full list of all the functions that are available.

Here's an example of using one of the mathematical functions `ROUND`, to round a value and return the result:

```sql
SELECT ROUND(1.2345678, 3);
-- Returns 1.235
```

There's a separate documentation page for all of the mathematical functions Let's look at some of the ways that you can use SQLite functions. The `INSTR` function will search for the first occurrence of one string within another. Here, we're finding the first occurrence of a space character within the **BillingAddress**.

```sql
SELECT INSTR(BillingAddress, ' ') FROM Invoice LIMIT 25;
```

You can combine functions, even nesting them within one another. Let's use the results of the `INSTR` function with another function:

```sql
SELECT SUBSTR(BillingAddress, 0, INSTR(BillingAddress, ' ')) FROM Invoice LIMIT 25;
-- SUBSTR(string, start_position, length)
```

The `INSTR` function finds the position of the first space (' ') in the **BillingAddress** column. Then, the `SUBSTR` function takes that position and retrieves everything from the start of the **BillingAddress** up to (but not including) that first space. This way, we get just the first word of the BillingAddress. The query is set to only show the results for the first 25 invoices.

You can apply a scaler function to an aggregate function, such as the `SUM` function you saw in earlier:

```sql
SELECT PRINTF('$%f', SUM(Total)) FROM Invoice;
```

This function call applies a format string to the `SUM` of all **Invoice** totals and Returning a dollar formatted result. The `$` is a literal dollar sign, and the `%f` indicates that we want to format it as a floating point number. Monetary sums are generally carried to two decimal places. Let's modify the `%f` syntax by inserting a `.2` in the middle:

```sql
SELECT PRINTF('$%.2f', SUM(Total)) FROM Invoice;
```

This will cause `PRINTF` to always return two decimal places and no more than two. See this part of the [SQLite documentation](https://sqlite.org/printf.html) for more details.

Although SQLite doesn't have _date_, _time_, or _datetime_ data types, you can still work with dates that are contained in text columns. For example, you can use the strftime function to reformat a text column that's containing a date time value:

```sql
-- Retrieve the first 10 invoice dates and their formatted versions
SELECT InvoiceDate, strftime('%Y-%m-%d', InvoiceDate) FROM Invoice limit 10;
```

This will pare it down to just the date portion, and we will see the original **InvoiceDate** plus the modified value side by side. You can check out the date and time function documentation on [this page](https://www.sqlite.org/lang_datefunc.html).

#### Aggregate Functions

Earlier we saw how to use the `GROUP BY` clause with the `SUM` and `COUNT` functions. These are examples of aggregate functions as shown here on the [documentation page](https://www.sqlite.org/lang_aggfunc.html). Let's take a look at these and other functions in more detail. You can use the `COUNT` function to count the number of values:

```sql
-- Returns the total number of invoices in the Invoice table.
SELECT COUNT(*) FROM Invoice;

-- Returns the count of non-null BillingPostalCodes in the Invoice table.
SELECT COUNT(BillingPostalCode) FROM Invoice;
```

In the first form, you're counting the number of rows. However, in the second form, you're counting the number of non null values for a given value, in this case, the `BillingPostalCode` column. The results are different because there are a number of invoices without postal codes. You can also count the number of unique items by using the `DISTINCT` keyword:

```sql
-- Counts the number of unique billing postal codes in the Invoice table.
SELECT COUNT(DISTINCT BillingPostalCode) FROM Invoice;
```

This shows us all the unique postal codes in the **Invoice** table. What if we wanted to answer a somewhat complex question, such as what is the average number of customers per country? Well, we'd start with something like this:

```sql
-- This query counts the number of distinct customers in each country
SELECT Country, COUNT(DISTINCT CustomerId) AS num
FROM CUSTOMER
GROUP BY Country;
```

This will give us the number of customers in each country. Next, we would set that query up as a _subquery_ and take the average of num customers from that subquery:

```sql
-- Calculate the average number of distinct customers per country
SELECT AVG(num) FROM (SELECT Country, COUNT(DISTINCT CustomerId) AS num
    FROM CUSTOMER
    GROUP BY Country
);
```

This tells us that on average each country has roughly 2.4 customers. But why stop there? Average only tells us part of the story. What if we wanted to know the min and the max, as well? To do that, we can set up the same query and add two more aggregate calculations to the select statement, `MIN` and `MAX`:

```sql
-- Calculates the average, minimum, and maximum number of unique customers per country
SELECT AVG(num) AS avg_num,
       MIN(num) AS min_num,
       MAX(num) AS max_num
FROM (SELECT Country, COUNT(DISTINCT CustomerId) AS num
    FROM CUSTOMER
    GROUP BY Country
);
```

Another handy aggregate function is `GROUP_CONCAT`. You can use this to produce a comma separated list of aggregate values. Here, we're looking at the list of states represented within each country:

```sql
-- retrieves a list of countries along with a concatenated string of distinct states
-- for each country.
SELECT Country, GROUP_CONCAT(DISTINCT State) AS States
FROM Customer
GROUP BY Country;
```

Without that `DISTINCT`, we might see duplicates of some states.

#### Window Functions

QLite's window functions shown on the [documentation page](https://www.sqlite.org/windowfunctions.html) are similar to grouping and aggregation, except that they don't change the number of rows returned. Suppose you want a quick way to add a counter that goes from the largest column value to the smallest. You could use the `ROW_NUMBER` windowing function to do this:

```sql
-- Selects the genre names and assigns a sequential row number to each genre,
-- ordering them in descending order by name, and then orders the final
-- result set by the row numbers.
SELECT Name, ROW_NUMBER() OVER (ORDER BY Name DESC)
FROM Genre
ORDER BY 2;
```

The `OVER` clause defines the window over which the calculation is performed. In this case, it's the whole table sorted by `Name` descending. Even though the output of the query is sorted descending, the row number counter increases in the descending direction so we get the names in reverse alphabetic order. That too, in the order by clause is a numeric alias that refers to the second column in the output. You can also use aggregate functions with windows. Here's a query that lists `CustomerId`, `City` and `Country` as well as the number of customers per country:

```sql
-- Retrieves the CustomerId, City, and Country from the Customer table,
-- while calculating the number of customers per country using a window function.
SELECT CustomerId, City, Country, COUNT(CustomerId)
OVER (PARTITION BY Country) AS num_per_country
FROM Customer ORDER BY CustomerId;
```

It uses the `PARTITION` clause to define a separate window for each country in the table and performs a count of `CustomerId`s over each country.

> A window function is an SQL function where the input values are taken from a **window** of one or more rows in the results set of a `SELECT` statement.
>
> Window functions are distinguished from _scalar functions_ and _aggregate functions_ by the presence of an `OVER` clause. If a function has an `OVER` clause, then it is a window function. If it lacks an `OVER` clause, then it is an ordinary aggregate or scalar function. Window functions might also have a `FILTER` clause in between the function and the `OVER` clause.

#### Queries of Querie

We saw examples of _subqueries_ where one query is nested within another. Let's take a look at some of the ways you can use subqueries. Here's a subquery where you can find out the name of all `Tracks` that appear on at least one `Invoice`:

```sql
-- Retrieves the names of all tracks that have associated invoice lines.
SELECT Name FROM Track
WHERE TrackId IN (
    SELECT TrackId
    FROM InvoiceLine
);
```

The `IN` key word limits the result to **TrackId**s that are found in the subquery. You could reformulate that proceeding query as a `JOIN` but you need to use `DISTINCT` to avoid duplicate results. Otherwise, each name would appear once for each **Invoice** line item it's associated with:

```sql
SELECT DISTINCT t.Name
FROM Track t
JOIN InvoiceLine l
ON l.TrackId = t.TrackId;
```

Here's an example that would be complicated to achieve without a subquery:

```sql
SELECT Name FROM Track
WHERE TrackId NOT IN (
    SELECT TrackId
    FROM InvoiceLine
);
```

This looks for all tracks that haven't sold. In other words, all **Track**s that aren't associated with any **InvoiceLine** item. A _correlated subquery_ is one that refers to the parent query. This query lists all the tracks with the longest duration for each album:

```sql
SELECT t1.AlbumId, t1.Name, t1.MilliSeconds
FROM Track t1
WHERE t1.MilliSeconds = (
    SELECT MAX(t2.MilliSeconds)
    FROM Track t2
    WHERE t2.AlbumId = t1.AlbumId
);
```

Because the parent query and the subquery both consult the **Track** table, it's essential that we use table aliases, in this case, `t1` and `t2`, to keep track of which one is which. The results are similar to the windowing query you saw in before with one difference: If there's a tie, if two tracks have the same maximum length on an album, you would see both tracks in the results.

#### ACID compliant

Let’s start with the obvious: what does **ACID compliant** mean? The short answer is that **ACID**, an acronym for “**Atomicity**, **Consistency**, **Isolation**, and **Durability**” is a set of principles that ensure database transactions are processed reliably. When any data storage system upholds those principles, it is said to be _ACID compliant_.

When data integrity and reliability are top considerations in a transaction processing system which we will talk about them shortly, the system will typically apply four properties to those transactions for ACID-compliance:

- **Atomicity**: A transaction is treated as a single atomic unit. All steps that make up the transaction must succeed or the entire transaction rolls back. If they all succeed, the changes made by the transaction are permanently committed to the managing system. Consider the transfer transaction example. For the transaction to be committed to the database, the $200 must be successfully deducted from the savings account and added to the checking account, and the funds in both accounts must be verified to ensure their accuracy. If any of these tasks fail, all changes roll back and none are committed.
- **Consistency**: A transaction must preserve the consistency of the underlying data. The transaction should make no changes that violate the rules or constraints placed on the data. For instance, a database that supports banking transactions might include a rule stating that a customer's account balance can never be a negative number. If a transaction attempts to withdraw more money from an account than what is available, the transaction will fail, and any changes made to the data will roll back.
- **Isolation**: A transaction is isolated from all other transactions. Transactions can run concurrently only if they don't interfere with each other. Returning to the transfer transaction example, if another transaction were to attempt to withdraw funds from the same savings account, isolation would prevent the second transaction from firing. Without isolation, it might be possible for the second transaction to withdraw more funds than are available in the account after the first transaction was completed.
- **Durability**: A transaction that is committed is guaranteed to remain committed -- that is, all changes are made permanent and will not be lost if an event such as a power failure should occur. This typically means persisting the changes to nonvolatile storage. If durability were not guaranteed, it would be possible for some or all changes to be lost, affecting the data's reliability.

#### Transactions and Isolation

By default, SQLite operates in auto-commit mode. It means that for each command, SQLite starts, processes, and commits the transaction automatically.

To start a transaction explicitly, first, open a transaction by issuing the command below:

```sql
BEGIN TRANSACTION;
```

After executing that, the transaction is open until it is explicitly committed or rolled back. Second, issue SQL statements to select or update data in the database. Note that the change is only visible to the current session (or client).

Third, commit the changes to the database by using one of the statement below:

```sql
-- Both are the same
COMMIT;
COMMIT TRANSACTION;
```

If you do not want to save the changes, you can roll back using one of the statement below:

```sql
-- Both are the same
ROLLBACK;
ROLLBACK TRANSACTION;
```

Lets see some example. We will create two new tables: **accounts** and **account_changes** for the demonstration. The **accounts** table stores data about the account numbers and their balances. The **account_changes** table stores the changes of the accounts. First, create the tables by using the following `CREATE TABLE` statements:

```sql
CREATE TABLE accounts (
	account_no INTEGER NOT NULL,
	balance DECIMAL NOT NULL DEFAULT 0,
	PRIMARY KEY(account_no),
        CHECK(balance >= 0)
);

CREATE TABLE account_changes (
	change_no INT NOT NULL PRIMARY KEY,
	account_no INTEGER NOT NULL,
	flag TEXT NOT NULL,
	amount DECIMAL NOT NULL,
	changed_at TEXT NOT NULL
);
```

Second, insert some sample data into the **accounts** table:

```sql
INSERT INTO accounts (account_no,balance)
VALUES (100,20100);

INSERT INTO accounts (account_no,balance)
VALUES (200,10100);
```

Third, query data from the **accounts** table:

```sql
SELECT * FROM accounts;
```

Fourth, transfer 1000 from account 100 to 200, and log the changes to the table **account_changes** in a single transaction:

```sql
BEGIN TRANSACTION;

UPDATE accounts
   SET balance = balance - 1000
 WHERE account_no = 100;

UPDATE accounts
   SET balance = balance + 1000
 WHERE account_no = 200;

INSERT INTO account_changes(account_no,flag,amount,changed_at)
VALUES(100,'-',1000,datetime('now'));

INSERT INTO account_changes(account_no,flag,amount,changed_at)
VALUES(200,'+',1000,datetime('now'));

COMMIT;
```

As you can see, balances have been updated successfully.

Sixth, query the contents of the **account_changes** table:

```sql
SELECT * FROM account_changes;
```

Let’s take another example of rolling back a transaction. First, attempt to deduct 20,000 from account 100:

```sql
BEGIN TRANSACTION;

UPDATE accounts
   SET balance = balance - 20000
 WHERE account_no = 100;

INSERT INTO account_changes(account_no,flag,amount,changed_at)
VALUES(100,'-',20000,datetime('now'));
```

SQLite issued an error due to not enough balance:

```sql
[SQLITE_CONSTRAINT]  Abort due to constraint violation (CHECK constraint failed: accounts)
```

However, the log has been saved to the **account_changes** table. Query that table to see it yourself. Second, roll back the transaction by using the `ROLLBACK` statement:

```sql
ROLLBACK;
```

Finally, query data from the **account_changes** table, you will see that the change no #3 is not there anymore.
