# PHP Cookies Part 1
Cookies are small pieces of data stored on the client's browser. They are often used to track user activity and preferences.

## Setting a Cookie
```php
setcookie("username", "JohnDoe", time() + (86400 * 30), "/"); // 86400 = 1 day
```

## Retrieving a Cookie
```php
if(isset($_COOKIE["username"])) {
    echo "User: " . $_COOKIE["username"];
} else {
    echo "Cookie is not set.";
}
```

## Deleting a Cookie
```php
setcookie("username", "", time() - 3600, "/");
```

---

# PHP Sessions Part 2
Sessions are a way to store information across multiple pages for a user. Unlike cookies, session data is stored on the server.

## Starting a Session
```php
session_start();
$_SESSION["user"] = "JohnDoe";
```

## Accessing Session Data
```php
session_start();
echo "User: " . $_SESSION["user"];
```

## Destroying a Session
```php
session_start();
session_unset();
session_destroy();
```

---

# PHP Sessions without Cookies - Part 3
By default, PHP sessions use cookies. However, we can pass session IDs through URLs to maintain sessions without cookies.

## Starting a Session without Cookies
```php
ini_set("session.use_cookies", 0);
ini_set("session.use_only_cookies", 0);
session_start();
$_SESSION["user"] = "JohnDoe";
```

## Passing Session ID in URL
```php
<a href="page2.php?".session_name()."=".session_id()."">Next Page</a>
```

---

# PHP Cookies Code Walkthrough
This section demonstrates the complete implementation of cookies in PHP.

```php
<?php
// Setting a cookie
setcookie("username", "JohnDoe", time() + (86400 * 30), "/");

// Checking if the cookie exists
if(isset($_COOKIE["username"])) {
    echo "Welcome, " . $_COOKIE["username"];
} else {
    echo "No cookie found.";
}

// Deleting the cookie
setcookie("username", "", time() - 3600, "/");
?>
