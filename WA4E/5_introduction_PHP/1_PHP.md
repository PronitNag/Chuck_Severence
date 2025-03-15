# PHP Language Overview - Part 1

PHP (Hypertext Preprocessor) is a widely-used open-source scripting language suited for web development and can be embedded into HTML.

## Key Features:
- Server-side scripting
- Cross-platform compatibility
- Open-source and community-driven
- Supports databases like MySQL, PostgreSQL, etc.
- Efficient and flexible for web applications

## Usage:
PHP scripts are executed on the server, and the output is sent to the client’s browser. A basic example:
```php
<?php
    echo "Hello, World!";
?>
```

---

# Bonus: Rasmus Lerdorf Interview on Creating PHP

Rasmus Lerdorf, the creator of PHP, initially developed it as a set of Common Gateway Interface (CGI) scripts to track visits to his resume online. Over time, it evolved into a full-fledged scripting language.

### Key Points from the Interview:
- PHP was never intended to become a programming language.
- It gained popularity due to its simplicity and effectiveness in web development.
- The community played a significant role in shaping PHP into what it is today.
- PHP continues to evolve with modern web technologies.

---

# PHP Language - Variables and Constants - Part 2

## Variables:
- Declared using `$` followed by a name.
- Dynamically typed (no need to declare the type explicitly).
- Example:
  ```php
  <?php
      $name = "John";
      $age = 25;
      echo "Name: $name, Age: $age";
  ?>
  ```

## Constants:
- Defined using `define()` or `const` keyword.
- Once set, they cannot be changed.
- Example:
  ```php
  <?php
      define("SITE_NAME", "MyWebsite");
      echo SITE_NAME;
  ?>
  ```

---

# PHP Language - Expressions - Part 3

Expressions are combinations of variables, operators, and function calls that evaluate to a value.

## Types of Expressions:
1. **Arithmetic Expressions**:
   ```php
   <?php
       $sum = 5 + 10;
       echo $sum; // Outputs 15
   ?>
   ```
2. **String Expressions**:
   ```php
   <?php
       $greeting = "Hello" . " World!";
       echo $greeting; // Outputs "Hello World!"
   ?>
   ```
3. **Logical Expressions**:
   ```php
   <?php
       $isAdult = (18 >= 18) && (21 > 18);
       var_dump($isAdult); // Outputs bool(true)
   ?>
   ```

---

# PHP Language - Control Structures - Part 4

Control structures allow flow control in PHP scripts.

## Conditional Statements:
- **if-else**:
  ```php
  <?php
      $age = 20;
      if ($age >= 18) {
          echo "You are an adult.";
      } else {
          echo "You are a minor.";
      }
  ?>
  ```
- **switch**:
  ```php
  <?php
      $day = "Monday";
      switch ($day) {
          case "Monday":
              echo "Start of the week!";
              break;
          case "Friday":
              echo "Weekend is coming!";
              break;
          default:
              echo "A regular day.";
      }
  ?>
  ```

## Looping Structures:
- **for loop**:
  ```php
  <?php
      for ($i = 1; $i <= 5; $i++) {
          echo "Number: $i ";
      }
  ?>
  ```
- **while loop**:
  ```php
  <?php
      $x = 1;
      while ($x <= 5) {
          echo "Number: $x ";
          $x++;
      }
  ?>
  

