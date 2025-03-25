# PHP Arrays

Arrays in PHP are used to store multiple values in a single variable. PHP supports three types of arrays:

## 1. Indexed Arrays
Indexed arrays have a numeric index starting from 0.

```php
// Creating an indexed array
$fruits = array("Apple", "Banana", "Orange");

// Accessing elements
echo $fruits[0]; // Outputs: Apple
```

## 2. Associative Arrays
Associative arrays use named keys instead of numeric indexes.

```php
// Creating an associative array
$person = array("name" => "John", "age" => 30, "city" => "New York");

// Accessing elements
echo $person["name"]; // Outputs: John
```

## 3. Multidimensional Arrays
Multidimensional arrays contain one or more arrays inside them.

```php
// Creating a multidimensional array
$students = array(
    array("John", 25, "A"),
    array("Alice", 22, "B"),
    array("Bob", 23, "A")
);

// Accessing elements
echo $students[0][0]; // Outputs: John
```

## Array Functions
PHP provides various built-in functions to work with arrays:

- `count($array)`: Returns the number of elements in an array.
- `array_push($array, $value)`: Adds an element to the end of an array.
- `array_pop($array)`: Removes the last element from an array.
- `array_merge($array1, $array2)`: Merges two or more arrays.
- `array_keys($array)`: Returns all the keys of an array.

Example:

```php
$numbers = array(1, 2, 3);
array_push($numbers, 4);
print_r($numbers); // Outputs: Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 )
```

This covers the basics of PHP arrays. You can now commit this file to your GitHub repository.

