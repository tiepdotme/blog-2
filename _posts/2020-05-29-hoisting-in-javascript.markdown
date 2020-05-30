---
layout: post
title:  "Hoisting in JavaScript"
date:   2020-05-29 12:45:00 -0500
---

## What is hoisting?

Hoisting in JavaScript allows you to access variables and functions before they are created.

### Function Hoisting

```javascript
console.log(helloWorld()); // Hello World
console.log(multiplyNumbers(10, 5)); // 50

function helloWorld() {
    return "Hello World";
}

function multiplyNumbers(a, b) {
    return a * b;
}
```

You can see that I had used both the functions before they were declared, and JavaScript had no issues. It console logged the results as expected. It is a good practice to declare the functions prior to using them.

### Variable Hoisting

```javascript
console.log(age); // undefined
console.log(foo); // Reference Error: foo is not defined

var age = 10;
```

I was able to use the variable `age` before it being declared. It did log `undefined,` but it did not throw an error. If you compare it to the log of variable `foo,` you would see that the statement throws an error.


### Caveat of Hoisting
Why did it not log the value of the variable `age`?

> Hoisting supports variable and function **decelerations only**.

So when javascript hoists variables or functions, it takes the declaration to the top of the file. It does not, however, take the definition or the value assigned. So in the example of `age,` it hoisted the variable but not the value assigned to it.

Due to the principle of hoisting decelerations only, you would not have been able to use the example provided in function hoisting, if I used function expressions.

```javascript
console.log(helloWorld()); // helloWorld is not defined
console.log(multiplyNumbers(10, 5));

const helloWorld = function () {
    return "Hello World";
}

const multiplyNumbers = function (a, b) {
    return a * b;
}
```
