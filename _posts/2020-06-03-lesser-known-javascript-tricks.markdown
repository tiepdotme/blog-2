---
layout: post
title:  "Lesser-known JavaScript Tips & Tricks"
image: "/assets/social/js-tricks.png"
date:   2020-06-03 15:00:00 -0500
---

### Constructor brackets are optional

```javascript
const newDate = new Date(); // valid
const myClass = new MyClass(); // valid

const anotherDate = new Date; // Also valid
const myClass = new MyClass; // You bet this is valid
```

The only time you would need those brackets is if a constructor expects arguments.

### With statement

🚨`with` statement is not recommended, and it is forbidden in ES5 strict mode.

`with` statement extends the scope chain for a statement. `with` will add up all properties of an `object` passed in the scope chain.

```javascript
const person = {
    name: "Parwinder",
    age: 33,
    work: "Software Architect"
}

with (person) {
    console.log(`Hi, I am ${name}, and I am ${ age } years old. I work as a ${work}.`);
    // Hi, I am Parwinder, and I am 33 years old. I work as a Software Architect.
}
```

### Function arguments

Every function (except arrow functions) has an `arguments` array-like object that contains the value of all arguments passed to the function.

```javascript
function foo(a, b, c) {
  console.log(arguments[0]); // 1
  console.log(arguments[1]); // 2
  console.log(arguments[2]); // 3
}

foo(1, 2, 3);
```

`arguments` have two properties:

1. `arguments.callee`: the function being invoked
2. `arguments.callee.caller`: the function that has invoked the current function

🚨Just like the `with` statement above, `callee` and `caller` are forbidden in ES5 strict mode.

### Pure object

A pure object has no functions in its prototype.

```javascript
const x = {};
```

This creates an object, but the prototype will have a `constructor` and methods like `hasOwnProperty`, `isPrototypeOf` and `toString`.

```javascript
const x = Object.create(null);
```

`create(null)` generates an object with no prototype! 🤯

### Removing duplicates from an array

```javascript
const arr = [1, 1, 1, 1, 2, 3, 4, 5, 6, 7, 6, 6, 6, 7, 8, 9];
const arrWithoutDuplicates = [...new Set(arr)];
console.log(arrWithoutDuplicates); // [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```

The key property of a set is having unique values. Once we have the Set from an array, we can use the spread(...) operator to spread it into an empty array.

### Optional Chaining

Whenever you access a nested object property where you do not know if the sub object exists or not, you end up doing this:

```javascript
const nestedObject = {
    name: "Parwinder",
    details: {
        age: 33,
        cars: {
            first: "jeep",
            second: "tesla",
            accessories: {
                x: 200,
                y: 300
            }
        }
    }
}

if (nestedObject &&
    nestedObject.details &&
    nestedObject.details.cars &&
    nestedObject.details.cars.accessories) {
    console.log(nestedObject.details.cars.accessories.x); // 200
}
```

Optional chaining eliminates the clutter. With optional chaining you can do:

```javascript
const nestedObject = {
    name: "Parwinder",
    details: {
        age: 33,
        cars: {
            first: "jeep",
            second: "tesla",
            accessories: {
                x: 200,
                y: 300
            }
        }
    }
}

console.log(nestedObject?.details?.cars?.accessories?.x); // 200
```

🚨At the time of writing, `optional chaining` is in Stage 4 (Draft) of the new ES standard, and it will most likely make it to the final spec. 🤞
