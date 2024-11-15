#### Brief Background

Although the first official SQL specification was published in 1986 by the American National Standards Institute (ANSI), the language traces its roots back to the early 1970s and the pioneering relational database work that was being done at IBM. Current SQL standards are ratified and published by the International Standards Organization (ISO). Although a new standard is published every few years, the last significant set of changes to the core language can be traced to the SQL:1999 standard (also known as 
`SQL3`). Subsequent standards have mainly been concerned with storing and processing XML-based data. Overall, the evolution of SQL is firmly rooted in the practical aspects of database development, and in many cases new standards only serve to ratify and standardize syntax or features that have been present in commercial database products for some time. 

#### Declarative

SQL is a declarative language, meaning you describe the desired results and let the language determine how to achieve them. This differs from imperative languages like C, Java, or Python, where each step of an operation is explicitly defined. 

SQL’s original standards were designed to be accessible to non-programmers, which is why its syntax often resembles English. Most commands follow a verb-subject structure, such as `CREATE TABLE`, `DROP INDEX`, or `UPDATE table_name`. 

The declarative nature of SQL makes it easier to learn and remember, and the fixed structure allows databases to optimize queries more efficiently. However, SQL can feel limited when dealing with more complex operations, requiring nested queries or temporary tables. This is especially true for non-relational problems, which may not fit neatly into SQL's processing model.

Despite these limitations, SQL is a powerful tool for expressing complex operations. Once you understand its structure, you can often find simple solutions to complex problems, though it may require some adjustment if you’re used to imperative languages.

#### Basic Syntax

SQL consists of a number of different commands, such as `CREATE`, `TABLE` or `INSERT`. These 
commands are issued and processed one at a time. Each command implements a different action or feature of the database system. Although it is customary to use all capital letters for SQL commands and keywords, SQL is a _case-insensitive language_. All commands and keywords are case insensitive, as are identifiers (such as table names and column names). 
Identifiers must be given as literals. If necessary, identifiers can be enclosed in the standards compliant double-quotes `" "` to allow the inclusion of spaces or other nonstandard characters in an identifier. SQLite also allows identifiers to be enclosed in 
square brackets `[ ]` or back ticks `` `` for compatibility with other popular database products. SQLite reserves the use of any identifier that uses `sqlite_` as a prefix. 
SQL is whitespace insensitive, including line breaks. Individual statements are separated by a semicolon. If you’re using an interactive application, such as the `sqlite3` command-line tool, then you’ll need to use a semicolon to indicate the end of a statement. The semicolon is not strictly required for single statements, however, as it is 
properly a statement separator and not a statement terminator. When passing SQL commands into the programming API, the semicolon is not required unless you are passing more than one command statement within a single string. 
Single-line comments start with a double dash `--` and go to the end of the line. SQL 
also supports multi-line comments using the C comment syntax `/* */`.

Text literals are enclosed in single quotes `' '`. To represent a string literal that includes a single quote character, use two single quotes in a row `publisher = 'O''Reilly'`. C-style backslash escapes `\' ` are not part of the SQL standard and are not supported by SQLite.

> Text literals use single quotes. Double quotes are reserved for identifiers (table names, columns, etc.). C-style backslash escapes are not part of the SQL standard.

#### Simple Operators
 
SQLite supports the following unary prefix operators: 

- `- +`: These adjust the sign of a value. The `-` operator flips the sign of the value, effectively multiplying it by -1.0. The `+` operator is essentially a no-op, leaving a value 
with the same sign it previously had. It does not make negative values positive. 

- `~`: As in the C language, the `~` operator performs a bit-wise inversion. This operator 
is not part of the SQL standard. 

- `NOT`: The `NOT` operator reverses a Boolean expression using 3VL. 
There are also a number of binary operators. They are listed here in descending precedence.

- `||`: String concatenation. This is the only string concatenation operator recognized by the SQL standard. Many other database products allow `+` to be used for concatenation, but SQLite does not. 

- `+ - * / %`: Standard arithmetic operators for addition, subtraction, multiplication, division, and modulus (remainder).

- `| & << >>`: The bitwise operators or, and, and shift-high/shift-low, as found in the C language. 
These operators are not part of the SQL standard.

- `< <= => >`: Comparison test operators. Again, just as in the C language we have less-than, lessthan or equal, greater-than or equal, and greater than. These operators are subject 
to SQL’s 3VL regarding NULLs.

- `= == != <>`: Equality test operators. Both `=` and `==` will test for equality, while both `!=` and `<>` test for inequality. Being logic operators, these tests are subject to SQL’s 
3VL regarding NULLs. Specifically, value = NULL will always return NULL.

- `IN LIKE GLOB MATCH REGEXP`: These five keywords are logic operators, returning, true, false, or NULL state.

- `AND OR`: Logical operators. 

In addition to these basics, SQL supports a number of specific expression operations. For more information on these and any SQL-specific expressions, see the [official website](https://www.sqlite.org/lang_expr.html).
