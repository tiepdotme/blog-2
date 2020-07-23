---
layout: post
title: "Class Inheritance"
date: 2020-07-22 12:00:00 -0500
---

To create an instance of a class, we use the `new` keyword. If we would like to inherit from a base class, then we can use the `extends` keyword.

```javascript
class Person {
    name;
    age;

    constructor(name) {
        this.name = name;
        this.age = 33;
    }
    getName() {
        return this.name;
    }
}

// creating new instance of a class Person
const person = new Person("Parwinder");
console.log(person.getName()); // Parwinder
```

To extend the Person class, let's use `extend`.

```javascript
class Person {
    name;
    age;

    constructor(name) {
        this.name = name;
        this.age = 33;
    }
    getName() {
        return this.name;
    }
}

const person = new Person("Parwinder");
console.log(person.getName()); // Parwinder

class Female extends Person {
    gender;
    constructor(name) {
        super(name);
        this.gender = "Female";
    }
}

const female = new Female("Lauren");

console.log(female.getName()); // Lauren
console.log(female instanceof Female); // true
console.log(female instanceof Person); // true
```

`super` keyword allows us to use the parent class constructor. We can also use `super` to call a method from the parent/base class.

Classes in ES6 support:
* prototypical inheritance,
* `super`
* instance and static methods.


