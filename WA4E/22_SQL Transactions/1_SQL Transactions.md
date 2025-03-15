# SQL Transactions

## What is a Transaction?
A transaction in SQL is a sequence of one or more SQL statements that are executed as a single unit of work. A transaction follows the ACID properties to ensure data integrity and consistency.

## ACID Properties
1. **Atomicity**: Ensures that a transaction is either fully completed or fully rolled back.
2. **Consistency**: Ensures that a transaction brings the database from one valid state to another.
3. **Isolation**: Ensures that transactions are executed independently and do not interfere with each other.
4. **Durability**: Ensures that committed transactions are permanently saved in the database.

## SQL Transaction Control Statements
### 1. **BEGIN TRANSACTION**
Starts a new transaction.
```sql
BEGIN TRANSACTION;
```

### 2. **COMMIT**
Saves all changes made by the transaction permanently.
```sql
COMMIT;
```

### 3. **ROLLBACK**
Undoes all changes made by the transaction.
```sql
ROLLBACK;
```

### 4. **SAVEPOINT**
Creates a savepoint within a transaction to which you can later rollback.
```sql
SAVEPOINT savepoint_name;
```

### 5. **ROLLBACK TO SAVEPOINT**
Rolls back to a specific savepoint without affecting the rest of the transaction.
```sql
ROLLBACK TO SAVEPOINT savepoint_name;
```

### 6. **SET TRANSACTION**
Defines properties for the transaction (e.g., isolation level).
```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

## Example of a Transaction
```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 1000 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 2;

IF @@ERROR <> 0
    ROLLBACK;
ELSE
    COMMIT;
```

## Isolation Levels in Transactions
1. **READ UNCOMMITTED** - Allows reading uncommitted changes (dirty reads).
2. **READ COMMITTED** - Prevents dirty reads but allows non-repeatable reads.
3. **REPEATABLE READ** - Prevents dirty and non-repeatable reads.
4. **SERIALIZABLE** - Ensures complete isolation of transactions.

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

## Conclusion
SQL transactions help maintain data integrity by ensuring operations are executed in a reliable and consistent manner. Understanding and properly implementing transactions is crucial for database management.

