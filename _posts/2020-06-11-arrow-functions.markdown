---
layout: post
title:  "Arrow Functions: Basics"
image: "/assets/social/arrow.png"
date:   2020-06-11 12:00:00 -0500
---

## Introduction

Arrow functions introduced in ES6 is a concise way of creating functions compared to function expressions.

The name arrow function comes from the use of `=>`.

**Syntax**:

```javascript
const functionName = (arg1, arg2, ... argN) => {
    return value;
}
```

**Example**

```javascript
const multiply = (a, b) => {
    return a * b;
}

console.log(multiply(7, 8)); // 56
console.log(multiply(3, 2)); // 6
```

**Key Features**

1. Arrow functions are anonymous function until they are assigned to a variable.
2. If there is only 1 argument, we can skip parenthesis
   ```javascript
   const square = x => {
       return x * x;
   }

   console.log(square(2)); // 4
   console.log(square(7)); // 49
   ```
3. If there is no arguments, we need to have the parenthesis
   ```javascript
   const greeting = () => {
       return "Hello World!";
   }

   console.log(greeting()); // Hello World!
   ```
4. If the function has only one statement, and the statement returns a value, we can remove the brackets and the return keyword.
   ```javascript
   const greeting = () => "Hello World!";
   console.log(greeting()); // Hello World
   ```

Now that we know all these key features, let us rewrite the example to get the square of a number:

```javascript
const square = x => x * x;
console.log(square(4)); // 16
```
