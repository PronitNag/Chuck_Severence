# Basic JavaScript

## Introduction
JavaScript is a versatile, high-level programming language primarily used for web development. It allows developers to create dynamic and interactive web pages.

## Variables and Data Types
JavaScript provides various data types and ways to declare variables:

```js
// Variable Declaration
var name = "John"; // Old way
let age = 25; // Block-scoped variable
const PI = 3.1416; // Constant value

// Data Types
let str = "Hello, World!"; // String
let num = 10; // Number
let isActive = true; // Boolean
let arr = [1, 2, 3]; // Array
let obj = { key: "value" }; // Object
```

## Operators
JavaScript supports various operators:

```js
let sum = 5 + 10; // Arithmetic (+, -, *, /, %)
let isEqual = 10 === "10"; // Comparison (==, ===, !=, !==, >, <, >=, <=)
let andOperator = true && false; // Logical (&&, ||, !)
```

## Conditional Statements
Conditional statements control the flow of execution.

```js
let num = 10;
if (num > 5) {
    console.log("Number is greater than 5");
} else {
    console.log("Number is 5 or less");
}
```

## Loops
Loops are used to execute a block of code multiple times.

```js
// For Loop
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// While Loop
let j = 0;
while (j < 5) {
    console.log(j);
    j++;
}
```

## Functions
Functions allow code reusability.

```js
function greet(name) {
    return `Hello, ${name}!`;
}
console.log(greet("Alice"));
```

## Arrays and Objects
Arrays and objects help manage multiple values efficiently.

```js
// Arrays
let colors = ["red", "green", "blue"];
console.log(colors[0]); // Access first element

// Objects
let person = {
    name: "John",
    age: 30,
    greet: function () {
        console.log("Hello!");
    }
};
console.log(person.name);
person.greet();
```

## DOM Manipulation
JavaScript can be used to modify the Document Object Model (DOM).

```js
document.getElementById("myElement").innerText = "Hello, JavaScript!";
```

## Events
Event handling allows interactivity.

```js
document.getElementById("myButton").addEventListener("click", function () {
    alert("Button Clicked!");
});
```

## Asynchronous JavaScript
JavaScript supports asynchronous programming using callbacks, promises, and async/await.

```js
// Using Promises
fetch("https://api.example.com/data")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

## Conclusion
JavaScript is a powerful language for building web applications. Mastering its core concepts is essential for front-end and back-end development.

