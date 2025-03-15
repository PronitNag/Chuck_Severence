# PHP HTTP Redirects - Part 1

## Introduction
HTTP redirects are used to send users from one URL to another. In PHP, redirects are commonly implemented using the `header()` function.

## Basic Redirect
```php
header("Location: https://example.com");
exit();
```

## Types of Redirects
1. **301 Moved Permanently**
   ```php
   header("Location: https://example.com", true, 301);
   exit();
   ```
2. **302 Found (Temporary Redirect)**
   ```php
   header("Location: https://example.com", true, 302);
   exit();
   ```

---

# PHP Post / Redirect - Part 2

## Introduction
The Post/Redirect/Get (PRG) pattern is used to prevent form resubmission issues.

## Example
```php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Process form data
    header("Location: success.php");
    exit();
}
```

## Benefits
- Prevents duplicate form submissions
- Improves user experience

---

# PHP - Flash Messages / Authentication - Part 3

## Flash Messages
Flash messages are temporary messages stored in the session and displayed to users once.

### Example
```php
session_start();
$_SESSION['message'] = "Login successful!";
header("Location: dashboard.php");
exit();
```

## Authentication
User authentication ensures only authorized users access protected pages.

### Example
```php
session_start();
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit();
}
```

## Conclusion
Using sessions and redirects, we can manage authentication and flash messages effectively.

