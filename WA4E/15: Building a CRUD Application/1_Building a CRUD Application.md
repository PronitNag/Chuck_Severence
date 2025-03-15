# Building a Complete CRUD Application with LAMP Stack and PHP

## Introduction
In this guide, we will build our first complete application that includes multiple screens to **Create, Read, Update, and Delete (CRUD)** data. This project will integrate all the core concepts learned previously while utilizing the **LAMP stack** (Linux, Apache, MySQL, and PHP).

## Prerequisites
Before starting, ensure you have the following installed:
- **Linux** (or any OS with Apache and MySQL support)
- **Apache** Web Server
- **MySQL** Database
- **PHP** (Version 7+ recommended)
- A Text Editor (VS Code, Sublime Text, etc.)

## Project Setup
1. **Install LAMP Stack**
   ```bash
   sudo apt update
   sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql
   ```
2. **Start Services**
   ```bash
   sudo systemctl start apache2
   sudo systemctl start mysql
   ```
3. **Create a Database**
   ```sql
   CREATE DATABASE myapp;
   USE myapp;
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100),
       email VARCHAR(100) UNIQUE,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

## Application Structure
```
myapp/
│-- index.php  # Home Page (Read Data)
│-- create.php # Create New Record
│-- update.php # Update Existing Record
│-- delete.php # Delete a Record
│-- db.php     # Database Connection
│-- styles.css # Styling (Optional)
```

## CRUD Operations
### 1. **Database Connection (db.php)**
```php
<?php
$servername = "localhost";
$username = "root";
$password = "";
$database = "myapp";

$conn = new mysqli($servername, $username, $password, $database);
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```

### 2. **Create a New Record (create.php)**
```php
<?php
include 'db.php';
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST['name'];
    $email = $_POST['email'];
    $sql = "INSERT INTO users (name, email) VALUES ('$name', '$email')";
    $conn->query($sql);
    header("Location: index.php");
}
?>
<form method="POST">
    Name: <input type="text" name="name" required><br>
    Email: <input type="email" name="email" required><br>
    <button type="submit">Create</button>
</form>
```

### 3. **Read Data (index.php)**
```php
<?php
include 'db.php';
$result = $conn->query("SELECT * FROM users");
while ($row = $result->fetch_assoc()) {
    echo "<p>{$row['name']} ({$row['email']}) <a href='update.php?id={$row['id']}'>Edit</a> | <a href='delete.php?id={$row['id']}'>Delete</a></p>";
}
?>
```

### 4. **Update a Record (update.php)**
```php
<?php
include 'db.php';
$id = $_GET['id'];
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST['name'];
    $email = $_POST['email'];
    $conn->query("UPDATE users SET name='$name', email='$email' WHERE id=$id");
    header("Location: index.php");
}
$result = $conn->query("SELECT * FROM users WHERE id=$id")->fetch_assoc();
?>
<form method="POST">
    Name: <input type="text" name="name" value="<?php echo $result['name']; ?>" required><br>
    Email: <input type="email" name="email" value="<?php echo $result['email']; ?>" required><br>
    <button type="submit">Update</button>
</form>
```

### 5. **Delete a Record (delete.php)**
```php
<?php
include 'db.php';
$id = $_GET['id'];
$conn->query("DELETE FROM users WHERE id=$id");
header("Location: index.php");
?>
```

## Conclusion
This application provides a basic CRUD system using PHP and MySQL. You can enhance it further by adding:
- **Form Validation**
- **CSS Styling**
- **User Authentication**
- **AJAX for Asynchronous Operations**

Now, you can push this project to **GitHub** and deploy it on a server! 🚀

