# PHP, MySQL, and PDO

## 1. Introduction to PHP
PHP (Hypertext Preprocessor) is a popular server-side scripting language designed for web development. It is widely used for creating dynamic and interactive web applications.

## 2. Introduction to MySQL
MySQL is an open-source relational database management system (RDBMS) that stores and manages data efficiently. It is commonly used in web applications to store user data, product information, and more.

## 3. What is PDO (Portable Data Object)?
PDO (PHP Data Objects) is a database access layer that provides a uniform method for accessing multiple database types, including MySQL, PostgreSQL, and SQLite. It is preferred over MySQLi due to its flexibility and security features.

## 4. Connecting PHP with MySQL using PDO
To establish a connection between PHP and MySQL using PDO, use the following code:

```php
<?php
$dsn = 'mysql:host=localhost;dbname=testdb';
$username = 'root';
$password = '';

try {
    $pdo = new PDO($dsn, $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connected successfully";
} catch (PDOException $e) {
    echo "Connection failed: " . $e->getMessage();
}
?>
```

## 5. CRUD Operations using PDO
### a) Insert Data
```php
$sql = "INSERT INTO users (name, email) VALUES (:name, :email)";
$stmt = $pdo->prepare($sql);
$stmt->execute(['name' => 'John Doe', 'email' => 'john@example.com']);
```

### b) Fetch Data
```php
$sql = "SELECT * FROM users";
$stmt = $pdo->query($sql);
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
    echo $row['name'] . " - " . $row['email'] . "<br>";
}
```

### c) Update Data
```php
$sql = "UPDATE users SET email = :email WHERE name = :name";
$stmt = $pdo->prepare($sql);
$stmt->execute(['email' => 'new@example.com', 'name' => 'John Doe']);
```

### d) Delete Data
```php
$sql = "DELETE FROM users WHERE name = :name";
$stmt = $pdo->prepare($sql);
$stmt->execute(['name' => 'John Doe']);
```

## 6. Benefits of Using PDO
- Supports multiple databases
- Secure with prepared statements (prevents SQL injection)
- Better error handling
- More flexible and maintainable code

## 7. Conclusion
Using PHP with MySQL through PDO enhances database security and flexibility. Understanding PDO can help developers build scalable and secure web applications efficiently.

