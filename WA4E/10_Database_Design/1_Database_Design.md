# Data Modeling - Building a Data Model (Part 1)

## Introduction
Data modeling is the process of structuring and organizing data to be stored in a database. This includes defining entities, attributes, and relationships between different data points.

## Key Concepts
- **Entities**: Objects or concepts that store data (e.g., Customers, Orders, Products).
- **Attributes**: Characteristics of entities (e.g., Name, Price, Date of Purchase).
- **Primary Key**: A unique identifier for an entity.
- **Normalization**: Organizing data to reduce redundancy and improve integrity.

## Steps to Build a Data Model
1. Identify Entities
2. Define Relationships
3. Normalize Data
4. Validate Model
5. Implement in Database

---

# Data Modeling - Representing Relationships (Part 2)

## Introduction
Relationships define how entities interact with each other in a database.

## Types of Relationships
- **One-to-One (1:1)**: Each entity is related to only one other entity.
- **One-to-Many (1:M)**: One entity relates to multiple other entities.
- **Many-to-Many (M:M)**: Multiple entities relate to multiple other entities.

## Example
- A **Customer** can have multiple **Orders** (1:M relationship).
- An **Order** contains multiple **Products**, and a **Product** can be part of multiple **Orders** (M:M relationship).

---

# Data Modeling - Relationships in SQL (Part 3)

## Foreign Keys
A **foreign key** is a field in one table that uniquely identifies a row in another table.

### Example:
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(255)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

## Enforcing Relationships
- **Cascading Delete**: Automatically removes related records.
- **Cascading Update**: Updates related records when a primary key changes.

---

# Data Modeling - Using Join (Part 4)

## Introduction
SQL **JOIN** is used to combine records from multiple tables based on related columns.

## Types of Joins
- **INNER JOIN**: Returns matching records from both tables.
- **LEFT JOIN**: Returns all records from the left table and matching records from the right.
- **RIGHT JOIN**: Returns all records from the right table and matching records from the left.
- **FULL JOIN**: Returns all records from both tables.

### Example:
```sql
SELECT Customers.Name, Orders.OrderID 
FROM Customers 
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

---

# Data Modeling - Many-To-Many (Part 5)

## Handling Many-to-Many Relationships
Many-to-Many relationships require a **junction table** to break them into two **One-to-Many** relationships.

### Example:
```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    Name VARCHAR(255)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY
);

CREATE TABLE OrderDetails (
    OrderID INT,
    ProductID INT,
    PRIMARY KEY (OrderID, ProductID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);
```

## Benefits
- Prevents data duplication
- Maintains referential integrity
- Improves query performance

