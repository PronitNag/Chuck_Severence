# Single Table SQL - Part 1

## Introduction to Single Table SQL
Single Table SQL queries involve retrieving, filtering, and manipulating data within a single table using SQL commands. This is fundamental for database operations.

### Basic SQL Queries
#### SELECT Statement
```sql
SELECT column1, column2 FROM table_name;
```
Retrieves specific columns from a table.

#### Filtering Data with WHERE
```sql
SELECT * FROM table_name WHERE condition;
```
Filters data based on a condition.

#### Sorting Data with ORDER BY
```sql
SELECT * FROM table_name ORDER BY column_name ASC|DESC;
```
Sorts results in ascending or descending order.

#### Limiting Results with LIMIT
```sql
SELECT * FROM table_name LIMIT number;
```
Restricts the number of rows returned.

---

# Single Table SQL - Part 2

## Advanced Queries on a Single Table

### Aggregation Functions
#### COUNT, SUM, AVG, MIN, MAX
```sql
SELECT COUNT(*), SUM(column_name), AVG(column_name), MIN(column_name), MAX(column_name)
FROM table_name;
```
Used to perform calculations on a set of values.

### Grouping Data with GROUP BY
```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name;
```
Groups rows with the same values.

### Filtering Groups with HAVING
```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 1;
```
Filters grouped results.

### Conditional Logic with CASE
```sql
SELECT column_name,
       CASE 
           WHEN condition THEN 'Value1'
           ELSE 'Value2'
       END AS alias_name
FROM table_name;
```
Adds conditional logic to queries.

### Updating and Deleting Data
#### UPDATE Statement
```sql
UPDATE table_name SET column_name = 'new_value' WHERE condition;
```
Modifies existing records.

#### DELETE Statement
```sql
DELETE FROM table_name WHERE condition;
```
Removes records based on a condition.

# SQLite in PHP: Creating a Table, Inserting Data, and Displaying Records

## Overview
This guide covers how to use SQLite with PHP to:
1. Create a database and a table.
2. Insert data into the table.
3. Retrieve and display data in an HTML table.

## Prerequisites
- PHP installed on your system.
- SQLite extension enabled (`sqlite3` is built into PHP 5+).
- A web server (e.g., Apache with PHP or a local development server like XAMPP).

## Full PHP Code
```php
<?php
// Step 1: Connect to the database (or create it if it doesn't exist)
$db = new SQLite3('database.db');

// Step 2: Create the table if it does not exist
$query = "CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    age INTEGER
)";
$db->exec($query);

// Step 3: Insert some data into the table
$insertQuery = "INSERT INTO users (name, age) VALUES
    ('Alice', 25),
    ('Bob', 30),
    ('Charlie', 28)";
$db->exec($insertQuery);

// Step 4: Retrieve and display the data
$result = $db->query("SELECT * FROM users");

echo "<h2>Users Table Data:</h2>";
echo "<table border='1'>
<tr><th>ID</th><th>Name</th><th>Age</th></tr>";

while ($row = $result->fetchArray(SQLITE3_ASSOC)) {
    echo "<tr><td>{$row['id']}</td><td>{$row['name']}</td><td>{$row['age']}</td></tr>";
}

echo "</table>";
?>
```

## Explanation
### 1. **Connecting to SQLite Database**
```php
$db = new SQLite3('database.db');
```
- This line opens or creates a SQLite database file named `database.db`.
- If the file doesn't exist, SQLite automatically creates it.

### 2. **Creating the Table**
```php
$query = "CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    age INTEGER
)";
$db->exec($query);
```
- `CREATE TABLE IF NOT EXISTS users` ensures the table is only created if it doesn't already exist.
- `id INTEGER PRIMARY KEY AUTOINCREMENT` creates a unique ID for each row.
- `name TEXT` stores names as text.
- `age INTEGER` stores age as a number.
- `$db->exec($query);` executes the SQL command.

### 3. **Inserting Data**
```php
$insertQuery = "INSERT INTO users (name, age) VALUES
    ('Alice', 25),
    ('Bob', 30),
    ('Charlie', 28)";
$db->exec($insertQuery);
```
- Inserts three sample records (Alice, Bob, and Charlie) into the `users` table.

### 4. **Retrieving and Displaying Data**
```php
$result = $db->query("SELECT * FROM users");
```
- Fetches all records from the `users` table.

```php
echo "<h2>Users Table Data:</h2>";
echo "<table border='1'>
<tr><th>ID</th><th>Name</th><th>Age</th></tr>";
```
- Prints an HTML table header.

```php
while ($row = $result->fetchArray(SQLITE3_ASSOC)) {
    echo "<tr><td>{$row['id']}</td><td>{$row['name']}</td><td>{$row['age']}</td></tr>";
}
```
- Loops through the results and prints each row inside a `<tr>` table row.

```php
echo "</table>";
```
- Closes the table tag after displaying all rows.

## Expected Output (in Browser)
| ID  | Name    | Age |
|-----|--------|-----|
| 1   | Alice   | 25  |
| 2   | Bob     | 30  |
| 3   | Charlie | 28  |

## Next Steps
- Accept user input using `$_POST` and insert dynamic values.
- Add **update** and **delete** functionality.
- Use **prepared statements** for better security against SQL injection.
- Integrate with a front-end UI for better user experience.

Let me know if you need help with any of these! 😊


