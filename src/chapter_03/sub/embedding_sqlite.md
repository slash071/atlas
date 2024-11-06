#### Programming Languages

Most programming languages have a well-supported API for working with SQLite. In some cases, you'll need to install a third party library but in many cases you might find it already built in. For example, SQLite is part of the Python Standard Library, so if you have a recent version of Python you're good to go.

You'll need to import the SQLite3 library, then open a database connection, and once you've done that, you can create a cursor object which you'll then use to execute statements. Here's an example of creating a table and running a `SELECT` statement to getting the results:

```python
import sqlite3

# Connect to SQLite database (or create it if it doesn't exist)
conn = sqlite3.connect('example.db')
cursor = conn.cursor()

# Create a table
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER NOT NULL
)
''')

# Insert some values into the table
cursor.execute('''
INSERT INTO users (name, age) VALUES
('Alice', 30),
('Bob', 25),
('Charlie', 35)
''')

# Commit the changes
conn.commit()

# Query the table
cursor.execute('SELECT * FROM users')
rows = cursor.fetchall()

# Display the results
print("Users in the database:")
for row in rows:
    print(row)

# Close the connection
conn.close()

```

As with other programming languages, you need to open the database from your code and then you can issue queries. You'll find SQLite support for many programming languages.
