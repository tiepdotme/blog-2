---
layout: post
title: "Classes, Not Just Syntactic Sugar"
image: "/assets/social/classes-sugar.png"
date: 2020-07-20 12:00:00 -0500
---

We went over classes in the last blog post and how they make it easier to create objects using a blueprint. The `class` keyword was introduced in ES2015/ES6, and there is common misconception that classes are barely syntactic sugar and nothing more. Classes are the core fundamentals of object-oriented programming. My aim with this blog post is to demystify the misconception and showcase how classes are slightly different from functions paired with `new` keyword.

### What does a class do?

```javascript
class EmployeeRecord {
    name = "New User";
    id = 0;

    constructor(firstName, lastName, id) {
        this.name = `${firstName} ${lastName}`;
        this.id = id;
    }

    reverseName() {
        return this.name.split("").reverse().join("");
    }
}

const employee1 = new EmployeeRecord("Parwinder", "Bhagat", 1);
const employee2 = new EmployeeRecord("Lauren", "L", 2);

console.log(employee1.name); // Parwinder Bhagat
console.log(employee2.name); // Lauren L
console.log(employee1.reverseName()); // tagahB redniwraP
```

In the above `class` example:

1. Under the hood, a function called `EmployeeRecord` is created. The function body is made of the constructor of the class. If there is no constructor, then the function body is empty.
2. All the class methods are stored on the prototype of `EmployeeRecord`.

With that logic, we can re-write the above class without using classes or the `class` keyword.

```javascript
function EmployeeRecord(firstName, lastName, id) {
    this.name = `${firstName} ${lastName}`;
    this.id = id;
}

EmployeeRecord.prototype.reverseName = function () {
    return this.name.split("").reverse().join("");
}

let employee1 = new EmployeeRecord("Parwinder", "Bhagat", 1);
const employee2 = new EmployeeRecord("Lauren", "L", 2);

console.log(employee1.name); // Parwinder Bhagat
console.log(employee2.name); // Lauren L
console.log(employee1.reverseName()); // tagahB redniwraP
```

The results are the same and this is where `class` is just syntactic sugar comes from.

### How do classes differ?

* There is a specific function kind assigned to classes and it is checked at multiple places most importantly whenever we instantiate a class.

```javascript
class EmployeeRecord {
    constructor() { }
}

console.log(typeof EmployeeRecord); // function
EmployeeRecord(); // Value of type 'typeof EmployeeRecord' is not callable. Did you mean to include 'new'?
```

* Functional inheritance works using the prototype. Classes do the same using a cleaner syntax with `extends` keyword.

```javascript
class Person {
    sayName() {
        console.log("My name is Person");
    }

    sayAge() {
        console.log("I am 30 years old."); // I am 30 years old.
    }
}

class Employee extends Person {
    sayDepartment() {
        console.log("I work for the tech department."); // I work for the tech department.
    }

    sayHello() {
        console.log("Hi, I am the new employee"); // Hi, I am the new employee
    }
}

let employee = new Employee;

employee.sayHello();
employee.sayAge();
employee.sayDepartment();

console.log(employee instanceof Person); // true
console.log(employee instanceof Employee); // true
```

* Function declarations are hoisted, and class declarations are not!

```javascript
const employee = new Employee(); // ReferenceError or Employee is not a constructor

class Employee {
    constructor() {}
}
```

* A class always runs in strict mode. All code inside the class is automatically in strict mode.

* Function decelerations and expressions can be overridden as they are similar to a `var` whereas classes are not overridden. They are like `let` and `const` keywords, let does not allow multiple declarations with the same name inside its scope.

* Objects can have (non-enumerable) properties that do not show up when iterated through that object. Class methods are non-enumerable and have the enumerable property set to false. If we use `for..in` to loop over an object from a class, we will not get the methods.
