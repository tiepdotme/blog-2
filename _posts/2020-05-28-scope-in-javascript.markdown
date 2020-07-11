---
layout: post
title:  "Scope in JavaScript: Global vs. Local vs. Block"
image: "/assets/social/scope.png"
date:   2020-05-28 14:45:00 -0500
---

## Scope

Scope in JavaScript defines where a declared variable or function is available. Depending on where the declaration happened, a variable or a function may be available to individual code blocks in JavaScript and not other. We deal with three types of scopes in JavaScript.

### Global Scope

When a variable/function is declared in a script tag or a JS file (outside all functions)
**and**
accessible from everywhere within that file or the HTML document, it has been declared in the global scope. It is not inside a function or a module.

In the case of a browser, there is a global variable available called `window`. You will see developers swapping `window.setinterval` or `setInterval` without much consequence or side effect. That is because `setInterval` is in the global scope. There are plenty of other methods that the browser provides on the `window` object.

> There are three operators used to declare variables. `const`, `let` and `var`. All three can be used to declare global variables, but only `var` attaches to the `window` scope.

### Function or Local Scope

When a variable is declared inside a function, it is only available within that function and not outside it. This is because the variable is locally scoped to that function.

```javascript
const first = "Parwinder"

function sayLastName() {
    const last = "Bhagat";
}

console.log(first); // Parwinder
console.log(last); // Reference Error: last is not defined
```

You can see that the variable `last` is not accessible outside the function. I can access it inside the function `sayLastName` without an issue.

```javascript
const first = "Parwinder"

function sayLastName() {
    const last = "Bhagat";
    console.log(last); // Bhagat
}

sayLastName(); // Remember, I have to call a function to execute the code block in it.

console.log(first); // Parwinder
```

We read about global variables and how they are available everywhere. This applies to the above snippet too.

```javascript
const first = "Parwinder"

function sayLastName() {
    console.log(first); // Parwinder
    const last = "Bhagat";
    console.log(last); // Bhagat
}

sayLastName();

console.log(first); // Parwinder
```

Even though I could not access `last` outside the function, I can still access the **global variable** `first` inside the function. This works in JavaScript because when the interpreter reads a variable, it will look for the declaration of that variable in the scope. If the variable is not found in the scope, it goes up one scope. In this example, when we log `first` inside `sayLastName`, JS engine will look for it in the function scope and then in the global scope. If it doesn't find it in either, it will go up to the window scope when running JS in a browser. If the variable is not found in any scope going up, we get a reference error thrown.

### Block Scope
A block could be defined as a piece of code encapsulated between two curly braces. By this definition, a function is a block. There could be smaller blocks in a function, though. A `for loop` or an `if` statement inside a function will have its block scope. Block scope helps us to form the small scope segment in code.

The way to create a block scope is by using the `const` or `let` keyword to declare variables.

```javascript
function amICool(name) {
    let attribute;
    if (name === "Parwinder") {
        attribute = "cool"
    }
    return attribute;
}

console.log(amICool("Parwinder")); // cool
console.log(amICool("Bhagat")); // undefined because the name was not "Parwinder"
```

The above example made the variable `attribute` function scope (or the block of the function). We can narrow this scope further.

```javascript
function amICool(name) {
    if (name === "Parwinder") {
       let attribute = "cool"
    }
    return attribute; // Reference error: attribute is not defined
}

console.log(amICool("Parwinder")); // Reference error: attribute is not defined
console.log(amICool("Bhagat")); // This will not run as we ran into an error with the first execution of the function
```

What happened here? `let` made the `attribute` variable block-scoped, and this time the block was limited to the `if` statement. When we try to return the variable outside the `if` statement, it cannot be found or accessed.

> As a rule of thumb, use `const` for everything. If the value of the variable changes later, then only switch it to `let`. Use `var` as a last resort.

### Lexical Scoping

Wait; what? I thought there were only three types of scopes. Lexical scoping is the scope model that is used by JavaScript. It works by defining the scope where a variable or function is declared and not where it is called from.

```javascript
const myName = "Parwinder";

function sayMyName() {
    console.log(myName);
}

function sayMyNameAgain() {
    const myName = "Bhagat";
    sayMyName();
}

sayMyNameAgain();
```

The output will be **Parwinder**. Why is that:

1. `sayMyNameAgain` is called which in turn calls `sayMyName`.
2. Although `sayMyNameAgain` has a function variable called `myName` with the value of **Bhagat**, it is never logged.
3. `sayMyName` logs the global variable `myName` with the value of **Parwinder**.

Because `sayMyName` executes with the scope of where it was declared.
