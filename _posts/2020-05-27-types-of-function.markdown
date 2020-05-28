---
layout: post
title:  "Types of Function in JavaScript"
date:   2020-05-27 14:45:00 -0500
---

## Declaration, Expression, Immediately Invoked (IIFE) and Arrow Functions

### Function declarations

When you use the `function` keyword to declare a named function and do not assign it to another variable, it is a function declaration.

```javascript
function greet(firstName = "new", lastName = "user") {
    return `Hello ${firstName} ${lastName}`;
}

console.log(greet("Parwinder", "Bhagat")); // Hello Parwinder Bhagat
console.log(greet()); // Hello new user
```

`greet` is an example of a function declaration. You see that in my first function invocation, I am passing the required arguments to the function. It works even without those arguments in the second invocation. This is called **default values**. You would do this when the caller of the function could pass you no value for an argument. When this happens, the function falls back on default values.

### Function expression

When a function is assigned to a named variable, it is called a function expression. When using a function express, we mostly use an anonymous function (no name for the function).

```javascript
const greet = function (firstName = "new", lastName = "user") {
    return `Hello ${firstName} ${lastName}`;
}

console.log(greet("Parwinder", "Bhagat")); // Hello Parwinder Bhagat
console.log(greet()); // Hello new user
```

Function declaration and expressions work almost identical in most situations. Function decelerations are loaded before any code is executed, whereas expressions are loaded only when the JavaScript interpreter reaches that line of code. This happens because of the principle of [hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) in JavaScript. It is a bit of an advanced topic, and we will discuss it in a future blog post.

### Immediately invoked function expressions (IIFE)

IIFE is a function that is declared and invoked at the same time. You create them by using anonymous functions and enclosing the function in round brackets `()`. You can then invoke them by merely calling the expression immediately with a followed pair of round brackets.

```javascript
(function(name){ // function expression enclosed in ()
    console.log(`Hello ${name}`); // Hello Parwinder
})("Parwinder"); // Immediately called by using () in the end. Yes we can pass arguments
```

### Arrow functions

An arrow function expression is a compact version of a regular function expression. The name comes from the symbol `=>` that is used in arrow functions.

```javascript
const hello = () => {
  return "Hello World!";
}

console.log(hello()); // Hello World
```

You can see that we have taken away the `function` keyword and added the `=>` symbol. We can make this more abbreviated.

```javascript
const hello = () => "Hello World!";
console.log(hello()); // Hello World
```

We have omitted the `return` keyword. This is entirely acceptable, and we can do this when a function has one statement, and that statement returns a value.

Arrow functions can also take arguments.

```javascript
const hello = (name) => `Hello ${name}`;
console.log(hello("Parwinder")); // Hello Parwinder
```

If you have only one parameter, the parentheses around it can be removed.

```javascript
const hello = name => `Hello ${name}`;
console.log(hello("Parwinder")); // Hello Parwinder
```

Arrow functions are not just prettier/compact versions of regular function expressions, but they also don't have their bindings to `this`, `arguments`, `super` or `new.target`. We will go over these principles of JS in the future blog posts.
