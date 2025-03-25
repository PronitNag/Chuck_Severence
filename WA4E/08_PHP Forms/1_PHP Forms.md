# PHP Forms Tutorial

## PHP Forms (Part 1)
PHP forms allow users to enter data and interact with web applications. Forms use the `<form>` element and various input fields to collect user input.

### Basic Form Structure
```html
<form action="process.php" method="post">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name">
    <input type="submit" value="Submit">
</form>
```

## PHP Forms - GET and POST (Part 2)
PHP provides two primary methods for handling form data: `GET` and `POST`.

- **GET Method**: Appends form data to the URL.
- **POST Method**: Sends data in the request body (more secure).

### Example
```php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST["name"];
    echo "Hello, " . htmlspecialchars($name);
}
```

## PHP Forms - Input Types (Part 3)
PHP forms support different input types like text, email, password, radio, checkbox, and more.

### Example
```html
<input type="email" name="email" required>
<input type="password" name="password">
<input type="radio" name="gender" value="male"> Male
<input type="radio" name="gender" value="female"> Female
```

## PHP Forms - HTML Injection and Validation (Part 4)
To prevent HTML injection and ensure secure data input, always validate and sanitize user inputs.

### Validation Example
```php
$name = trim($_POST["name"]);
$name = stripslashes($name);
$name = htmlspecialchars($name);
```

## PHP Forms - MVC (Part 5)
MVC (Model-View-Controller) pattern helps organize PHP applications for better scalability.

- **Model**: Handles database operations.
- **View**: Displays data to the user.
- **Controller**: Manages logic between Model and View.

### Example Structure
```
/app
   /models/User.php
   /views/form.php
   /controllers/FormController.php

