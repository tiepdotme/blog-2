---
layout: post
title: "What are, Mixins?"
image: "/assets/social/mixins.png"
date: 2020-07-23 12:00:00 -0500
---

### Introduction

Throughout all the blog posts on my blog, we have realized that prototypical inheritance works reasonably well in JavaScript. We do know; however, there is only one [[Prototype]] for each object. It means that an object can only inherit from one other object. The same goes for classes as we can only extend from one other class. JavaScript doesn't support multiple inheritance.

A mixin could is a class that has methods which we can use in our class without inheriting from the mixin class. It is a way of adding properties to object without using inheritance.

In theory, it would look something like this.

* Inheritance in classes

```javascript
class B extends A {}
```

* Inheritance but with Mixin (M1)

```javascript
class B extends A with M1 {}
```

* Inheritance with multiple Mixins (M1, M2, M3)

```javascript
class B extends A with M1, M2, M3 {}
```

The complete secret sauce of mixins is in `Object.assign`!

### Implementation

#### For Objects

```javascript
cconst employee = {
    name: "John Smith",
    age: 30,
    gender: "male"
}

const payroll = {
    duration: "monthly",
    amount: 7000,
    currency: "dollars"
}

const benefits = {
    retirement: true,
    savings: true,
    health: true,
    dental: false
}

const employeeProfile = Object.assign({}, employee, payroll, benefits);

console.log(employeeProfile);
```

The output on the console will be:

```javascript
{ name: 'John Smith',
  age: 30,
  gender: 'male',
  duration: 'monthly',
  amount: 7000,
  currency: 'dollars',
  retirement: true,
  savings: true,
  health: true,
  dental: false }
```

Yes, this is what a mixin does. It allows us to combine the properties of different objects into a single object (in simplest terms). The `Object.assign` copies all enumerable properties from one or more source objects to a target object. The first argument is the target object, followed by all source object(s).

#### For Classes

```javascript
let employeeDetails = {
    returnName() {
        console.log(`The employee is ${this.name}`);
    },
    subscribesToDental () {
        console.log(`Employee ${this.name} does ${(this.dental) ? "" : "not "}subscribe to dental benefits`);
    }
};

class Employee {
    name;
    dental;
    constructor(name, dental) {
        this.name = name;
        this.dental = dental;
    }
}

Object.assign(Employee.prototype, employeeDetails);

new Employee("Parwinder", false).returnName();
// The employee is Parwinder
new Employee("Parwinder", false).subscribesToDental();
// Employee Parwinder does not subscribe to dental benefits
new Employee("Robert", true).subscribesToDental();
// Employee Robert does subscribe to dental benefits
```

🚨 Javascript supports the use of `super` keyword now. Mixins are not able to support `super` as it is lexically bound!
