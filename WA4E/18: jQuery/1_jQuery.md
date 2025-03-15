# JavaScript and Object-Oriented Programming

JavaScript is a versatile language that supports multiple programming paradigms, including Object-Oriented Programming (OOP). JavaScript's OOP capabilities are implemented through:

## 1. Objects and Prototypes
- JavaScript is prototype-based, meaning objects inherit directly from other objects.
- Prototypes enable reuse of properties and methods without traditional class-based inheritance.

## 2. Constructor Functions
- Before ES6, constructor functions were commonly used to create objects.
- Example:
  ```js
  function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
      console.log(`Hello, my name is ${this.name}.`);
    };
  }
  let person1 = new Person("Alice", 25);
  person1.greet();
  ```

## 3. ES6 Classes
- ES6 introduced `class` syntax to make OOP more structured.
- Example:
  ```js
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
    greet() {
      console.log(`Hello, my name is ${this.name}.`);
    }
  }
  let person2 = new Person("Bob", 30);
  person2.greet();
  ```

## 4. Inheritance
- JavaScript supports class-based inheritance using `extends`.
- Example:
  ```js
  class Employee extends Person {
    constructor(name, age, job) {
      super(name, age);
      this.job = job;
    }
    work() {
      console.log(`${this.name} is working as a ${this.job}.`);
    }
  }
  let employee1 = new Employee("Charlie", 28, "Developer");
  employee1.greet();
  employee1.work();
  ```

## 5. Encapsulation
- JavaScript provides encapsulation through private fields and closures.
- Example:
  ```js
  class BankAccount {
    #balance;
    constructor(balance) {
      this.#balance = balance;
    }
    deposit(amount) {
      this.#balance += amount;
    }
    getBalance() {
      return this.#balance;
    }
  }
  let account = new BankAccount(1000);
  account.deposit(500);
  console.log(account.getBalance()); // 1500
  ```

## 6. Polymorphism
- JavaScript supports polymorphism through method overriding.
- Example:
  ```js
  class Animal {
    makeSound() {
      console.log("Some sound");
    }
  }
  class Dog extends Animal {
    makeSound() {
      console.log("Bark");
    }
  }
  let dog = new Dog();
  dog.makeSound(); // Output: Bark
  ```

## Conclusion
JavaScript's object-oriented capabilities allow developers to create modular, reusable, and maintainable code. Understanding these concepts is crucial for effective JavaScript development.

