---
layout: post
title: "Classes in JavaScript: Basics"
date: 2020-07-19 12:00:00 -0500
---

### Introduction

Developers working with JavaScript prefer various coding styles. It could be functional or object-oriented. Classes or the class construct is part of object-oriented programming. A class could be looked at as a template for creating objects. This template class is also helpful in setting initial values for members of the class and providing behaviors to certain other members (methods).

JavaScript is fully capable of providing such templating using functions and the `new` operator. This is the main reason most folks consider classes in JavaScript as syntactic sugar. It is not entirely true, and you will find that out as we progress through this JavaScript blog series.

🚨 Classes or the class keyword was introduced in ES2015/ES6.

### Syntax

```javascript
class EmployeeRecord {
    name = "New User";
    id = 0;

    constructor(firstName, lastName, id) {
        this.name = `${firstName} ${lastName}`;
        this.id = id;
    }
}

const employee1 = new EmployeeRecord("Parwinder", "Bhagat", 1);
const employee2 = new EmployeeRecord("Lauren", "L", 2);

console.log(employee1.name); // Parwinder Bhagat
console.log(employee2.name); // Lauren L
```

The class declaration is made using the `class` keyword, followed by the class name. The class body is between the two curly brackets. It is similar to a function declaration except for the missing arguments.

Arguments go to the constructor, which is used to initialize values when a class is instantiated. Think of it as a method in the class that runs as soon as you create a new object using the class. There can only be one unique method with the name "constructor" in a class.

### Class expression

If classes have decelerations, do they have expressions too (like functions)? You bet they do! These class expressions can be named or unnamed. Most folks tend to declare classes using the decelerations syntax.

```javascript
let EmployeeRecord = class {
    name = "New User";
    id = 0;

    constructor(firstName, lastName, id) {
        this.name = `${firstName} ${lastName}`;
        this.id = id;
    }
}

const employee1 = new EmployeeRecord("Parwinder", "Bhagat", 1);
const employee2 = new EmployeeRecord("Lauren", "L", 2);

console.log(employee1.name); // Parwinder Bhagat
console.log(employee2.name); // Lauren L
```

### Getters and setters

We use getters and setters in objects to retrieve properties or set value to properties. We can do the same with classes.

```javascript
let EmployeeRecord = class {
    firstName = "";
    lastName = "";

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    get employeeFullName() {
        return `${this.firstName} ${this.lastName}`;;
    }

    set employeeFirstName(firstName) {
        this.firstName = firstName;
    }

    set employeeLastName(lastName) {
        this.lastName = lastName;
    }
}

const employee1 = new EmployeeRecord("Parwinder", "Bhagat");
const employee2 = new EmployeeRecord("Lauren", "L");

// With a getter you do not need the () brackets
console.log(employee1.employeeFullName);
console.log(employee2.employeeFullName);

// A setter will prohibit someone from outside the class from changing the setter
employee1.employeeFullName = "Ricky Ricardo"; // Cannot assign to 'employeeFullName' because it is a read-only property.

// But we can use our helpful setter to do so!
// Keep in mind that a setter only takes one value

employee1.employeeFirstName = "Ricky";
employee1.employeeLastName = "Ricardo";

console.log(employee1.employeeFullName); // Ricky Ricardo

```

That's all for today! Watch out for the next blog post where we discuss how classes are not just syntactic sugar and how they differ from functions.

Happy coding 👋🏼
