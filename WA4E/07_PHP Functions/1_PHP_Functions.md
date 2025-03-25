# PHP Functions

## What is a Function in PHP?
A function in PHP is a block of reusable code that performs a specific task. It helps in code reusability, modularity, and maintainability.

### Syntax of a Function
```php
function functionName() {
    // Code to be executed
}
```

### Example
```php
function sayHello() {
    echo "Hello, World!";
}

sayHello(); // Calling the function
```

---

# PHP Functions - Scope and Modularity

## Scope of Variables in PHP
Scope determines where a variable can be accessed. PHP has three types of variable scope:

1. **Local Scope** - Variables declared inside a function are accessible only within that function.
2. **Global Scope** - Variables declared outside any function can be accessed globally using the `global` keyword inside a function.
3. **Static Scope** - Variables declared with `static` retain their value between function calls.

### Example of Variable Scope
```php
$globalVar = "I am global";

function testScope() {
    $localVar = "I am local";
    global $globalVar; // Accessing global variable inside function
    echo $globalVar;
}

// echo $localVar; // This will cause an error

testScope();
```

## Modularity in PHP
Modularity refers to breaking down a program into smaller, reusable functions. This improves code organization and reusability.

### Example of Modularity
```php
function add($a, $b) {
    return $a + $b;
}

function subtract($a, $b) {
    return $a - $b;
}

$result = add(5, 3);
echo "Sum: " . $result;

