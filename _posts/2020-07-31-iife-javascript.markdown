---
layout: post
title:  "Polymorphism in JavaScript"
date:   2020-07-30 12:00:00 -0500
---

### Immediately invoked function expressions (IIFE)

IIFE is a function that is declared and invoked at the same time. You create them by using anonymous functions and enclosing the function in round brackets `()`. You can then invoke them by merely calling the expression immediately with a followed pair of round brackets.

```javascript
(function(name){ // function expression enclosed in ()
    console.log(`Hello ${name}`); // Hello Parwinder
})("Parwinder"); // Immediately called by using () in the end. Yes we can pass arguments
```

Immediately invoked function expressions are helpful in:

* Avoiding variable hoisting from within blocks
* Not polluting the global scope
* Allowing us to access public methods while maintaining privacy for variables defined within the IIFE

In short, Immediately Invoked Function Expression is an excellent way to protect the scope of your function and the variables in it.

Just because I wrote the above function using the `function` keyword does not mean you have to. With ES6 popularity, you can use arrow functions as well.

```javascript
(name => {
    console.log(`Hello ${name}`); // Hello Parwinder
})("Parwinder");
```

Another way to create IIFE is by using the negation operator `!`. When we use the function keyword, what we are creating is a function declaration.

```javascript
function myName() {
    return "Parwinder";
}

console.log(myName()); // Parwinder
```

You have to invoke the declaration to the return eventually. If we prefix the function with negation, it becomes a function expression.

```javascript
!function myName() {
    return "Parwinder";
}
```

But this alone will not invoke it! It has only turned the function into an expression.

We must use `()` to call the method.

```javascript
!function myName() {
    console.log("Parwinder"); // Parwinder
}();
```

Ta-Da! Instead of creating a IIFE using `(function => {})()` we have done it using `!function => {}()`. No need to wrap our function block in `()`.

🚨 Do you see that I changed the return statement in my last example to a console.log? It is on purpose. IIFE will always return `undefined`. If we use the negation operator to create an IIFE, it will return `true` because `!undefined` is `true`.


