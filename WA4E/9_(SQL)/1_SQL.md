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

