---
layout: post
title:  "Optional Chaining"
date:   2020-06-16 12:00:00 -0500
---

### Introduction

🚨 Before you proceed, keep in mind that [optional chaining](https://github.com/tc39/proposal-optional-chaining) is not in the release on ECMAScript (yet). TC39 has it in [stage 4](https://tc39.es/process-document/) (Finished). You might need a polyfill for it.

Accessing object properties is a common occurrence in JavaScript. Plenty of times, these properties are nested. When you access a property on an object where the object is missing, JavaScript throws an error.

Let's go through some examples to understand it.

```javascript
const myObject = {
    name: "Parwinder",
    car: "Cybertruck",
    age: 42,
    computers: {
        first: {
            name: "iMac",
            year: 2017,
            spec: {
                cpu: "i7",
                ram: "16GB"
            }
        },
        second: {
            name: "MacBook Pro"
        }
    }
}

console.log(myObject.computers.first.spec.cpu); // i7
console.log(myObject.computers.second.spec.cpu); // Cannot read property 'cpu' of undefined
```

The first log goes through the object to get the CPU for my first computer. When you try doing the same for the second one, an error occurs.

We are trying to access the property `cpu` of `spec` when `spec` doesn't even exist. `cpu` is on the fifth level—anything before it can be null or undefined (myObject, computers, second, or spec).

We solve this problem by using `if` statement combined with AND `&&` operator.

```javascript
if(myObject && myObject.computers && myObject.computers.second && myObject.computers.second.spec) {
    console.log(myObject.computers.second.spec.cpu);
}
```

While the above script works, it is not the cleanest looking code. It's a lot of code to write to check for null/undefined of a property chain. Enter optional chaining.

Optional chaining is represented by `?.` and it can be used for keys and expressions on objects, array indexes, and functions available in an object.

### Optional chaining with object properties

We can rewrite the above `if` statement like below using optional chaining:

```javascript
myObject?.computers?.second?.spec?.cpu // undefined

// Concise and easy to read code
// or

myObject.computers.second.spec?.cpu
```

The `?.` operator short circuits an object property evaluation. Instead of returning an error by keeping on evaluating, optional chaining terminates as soon as it finds the first undefined/null in the chain and returns `undefined`.

`?.` only works where it is placed.

In ```myObject.computers.second.spec?.cpu```, the first three properties have to be present. Only `spec` is using optional chaining operator, and if `spec` does not exist, it will return `undefined.`

For any of the first three properties, an absence of the property will result in an error. So use `?.` only where something is optional and may be present or not. Using it everywhere could result in your code silently failing and harder to debug.

Example: If `computers` is optional, but `myObject` is mandatory, always opt got

`myObject.computers?.`

and not

`myObject?.computers?.`

### Optional chaining with object expressions

We can still use optional chaining if we use the literal or square bracket notation for accessing properties.

```javascript
console.log(myObject.computers.second?.["spec"].cpu); // undefined
console.log(myObject.computers.first?.["spec"].cpu); // i7
```

### Optional chaining with functions

Let us add a function to our object.

```javascript
const myObject = {
    name: "Parwinder",
    car: "Cybertruck",
    age: 42,
    getAge: function () {
        return this.age;
    }
}

console.log(myObject.getAge()); // 42
```

If the function could be optional, we can opt-in for optional chaining.

```javascript
console.log(myObject.getAge?.());
```

Optional chaining would ensure JavaScript does try to execute a function that does not exist. The invocation never happens due to the short circuit provided by `?.` operator.

### Optional chaining with arrays

```javascript
const names = ["Parwinder", "Lauren", "Leah", "Robert", "Eliu"];
console.log(names[3]); // Robert

const newEmployee = names?.[3]; // Check if names exists
console.log(newEmployee); // Robert
```

🚨 Optional chaining does not work on the left-hand side of an assignment.

```javascript
let myObject = {};
myObject?.name = "Parwinder"; // Uncaught SyntaxError
```
