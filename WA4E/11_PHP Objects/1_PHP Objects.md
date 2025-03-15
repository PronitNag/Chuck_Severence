# PHP Objects - Part 1

## Introduction to Objects in PHP
Objects are instances of classes, which act as blueprints for creating these objects. PHP, being an object-oriented language, provides full support for creating and managing objects.

### Key Concepts
- **Class:** A template for creating objects.
- **Object:** An instance of a class.
- **Property:** A variable inside a class.
- **Method:** A function inside a class.

---

# PHP Objects - Definition - Part 2

## Defining Classes and Objects in PHP
In PHP, classes and objects are defined using the `class` keyword. Objects are created using the `new` keyword.

### Example:
```php
class Car {
    public $brand;
    public function honk() {
        return "Beep Beep!";
    }
}

$myCar = new Car();
$myCar->brand = "Toyota";
echo $myCar->honk();
```

---

# PHP Objects - Life Cycle - Part 3

## Object Life Cycle in PHP
The life cycle of an object includes:
1. **Instantiation:** Creating an object from a class.
2. **Usage:** Modifying properties, calling methods.
3. **Destruction:** Releasing memory when the object is no longer needed.

### Example:
```php
class Demo {
    function __construct() {
        echo "Object Created!\n";
    }
    function __destruct() {
        echo "Object Destroyed!";
    }
}
$obj = new Demo();
```

---

# PHP Objects - Inheritance - Part 4

## Inheritance in PHP
Inheritance allows a class (child) to derive properties and methods from another class (parent).

### Example:
```php
class Animal {
    public function makeSound() {
        return "Some sound";
    }
}

class Dog extends Animal {
    public function makeSound() {
        return "Bark!";
    }
}

$dog = new Dog();
echo $dog->makeSound(); // Output: Bark!
