---
layout: post
title:  "What are objects & why are they important in JavaScript"
date:   2020-05-20 16:58:00 -0500
---

## Objects

### What are objects?

Objects are fundamental building blocks in JavaScript. Everything in JavaScript is expressed as an object or could be expressed as an object.

Objects are in the form of `key` and `value` pairs. Where a `key` is the reference to the `value`. Let me give you an example:

```javascript
const person = {
    firstName: "Parwinder",
    lastName: "Bhagat",
    age: 33,
    nicknames: ["Ricky", "P"],
    vehicles: {
        car: "BMW X5",
        motorcycle: "Honda Rebel 500"
    }
}
```

What I have done above is to declare an object called **person**. This person is a collection of data about an individual. An object starts and ends with `{}` curly braces. Within the curly braces, we have keys on the left (and they don't need to be put in quotes) and the values on the right of a semicolon.

The keys in the above object are *firstName*, *lastName*, *age*, *nickname*, *vehicles*, *car*, and *motorcycle*. The rest are values for those keys.

> **Important** Always keep in mind that objects are for data where the order does not matter. We cannot guarantee the order of keys in an object.

### How do we access object values?

To be able to access the object values you need to refer to the key you are looking for. Examples:

```javascript
console.log(person.firstName); // Parwinder
console.log(person.lastName); // Bhagat
console.log(person.age); // 33
console.log(person.nicknames); // ["Ricky", "P"]
console.log(person.vehicles.car); // BMW X5
```

You can also refer to the value by key literal instead of using the dot notation.

```javascript
console.log(person["age"]); // 33
```

### How do I assign value to keys in an object?

You can do it exactly how we assign various values to variables. And yes, you can assign any type of value to object keys. In my example above, I have assigned a string, number, array, and object to keys.

```javascript
person.firstName = "Julius";
person.lastName = "Caesar";
person.age = 48;
person.vehicles = null;
```

### What are valid keys in Objects?

You can use:

1. Strings E.g. "firstName"
2. Dashes in the middle of String, E.g. "last-Name"
3. Spaces in the middle of Strings, E.g. "middle name"
4. Numbers if they are put in quotes (technically making them strings), E.g. "007"

You cannot:

1. Start the object key with an underscore `_` even though objects have a hidden property that begins with underscore (\_\_proto__)

### How do I delete an object property?

Use the `delete` operator! 🙂

```javascript
delete(person.age);
console.log(person.name); // Julius
console.log(person.age); // undefined
```

### Can object keys be set to functions?

Absolutely!

```javascript
person.greeting = function(greeting = "Hola") {
    return `${greeting} ${this.firstName}`;
};
person.greeting("Salute"); // Salute Julius
person.greeting(); // Hola Julius
```
