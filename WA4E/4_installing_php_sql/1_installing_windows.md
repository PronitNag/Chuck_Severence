# PHP and SQL with MAMP on Windows

## 1. Introduction
MAMP (Macintosh, Apache, MySQL, PHP) is a free, local server environment that allows developers to build and test PHP applications with MySQL databases. Although it is named "MAMP," it is also available for Windows.

## 2. Installing MAMP on Windows
1. Download MAMP from the official website: [https://www.mamp.info/en/windows/](https://www.mamp.info/en/windows/)
2. Run the installer and follow the setup instructions.
3. Choose the installation directory (default is `C:\MAMP`).
4. Once installed, open MAMP and start the servers (Apache & MySQL).

## 3. Configuring MAMP
- The default document root is `C:\MAMP\htdocs`.
- To change the root directory:
  1. Open MAMP and go to **Preferences** → **Web Server**.
  2. Select a new document root.
- The default ports are:
  - Apache: `8888`
  - MySQL: `3306`
  
## 4. Running a PHP Script
1. Create a file `index.php` inside `C:\MAMP\htdocs`.
2. Add the following code:
   ```php
   <?php
   echo "Hello, PHP with MAMP!";
   ?>
   ```
3. Open a browser and go to `http://localhost:8888/index.php`.

## 5. Working with MySQL in MAMP
### Accessing phpMyAdmin
- Open `http://localhost:8888/phpMyAdmin/` in your browser.
- Default credentials:
  - Username: `root`
  - Password: `root`

### Creating a Database
1. Open phpMyAdmin.
2. Click **Databases** and enter a database name.
3. Click **Create**.

### Connecting PHP to MySQL
```php
<?php
$servername = "localhost";
$username = "root";
$password = "root";
$dbname = "test_db";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

echo "Connected successfully";
?>
```

## 6. Troubleshooting
- **Port Issues**: Change Apache/MySQL ports in **Preferences → Ports**.
- **PHP Version Issues**: Set the PHP version in **Preferences → PHP**.
- **MySQL Connection Errors**: Ensure MySQL is running and use correct credentials.

## 7. Conclusion
MAMP is a powerful tool for local PHP and MySQL development on Windows. By setting up MAMP properly, you can easily test and develop web applications before deploying them to a live server.
