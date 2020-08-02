---
layout: post
title:  "Pure Functions"
date:   2020-08-01 12:00:00 -0500
---

A pure [function](https://bhagat.me/blog/2020/05/26/functions-in-javascript.html) is a function that given the same input will provide the same output every time without causing any side effects. Pure functions are the building blocks of functional programming. They have a ton of advantages:

1. Due to their predictable behavior, they are easy to test.
2. Pure functions are independent of the outside state, reducing the number of bugs that could occur from external side effects.
3. They are great for parallel processing or distributed systems.
4. Since they are independent, they are easy to refactor, move around, reuse and reorganize.
5. They are useful in performance improvement using [memoization](https://bhagat.me/blog/2020/07/29/memoization.html). We can write a memoize function to memoize any pure function.

Let's start with a basic example of a pure function.

```javascript
const pure = (a, b) => a + b;
console.log(pure(5, 7)); // 12
console.log(pure(5, 7)); // 12
console.log(pure(5, 7)); // 12
```

The above is a simple function to add two numbers. It takes in `a` and `b`, adds them and returns the result. No matter how many times you pass it the same input, it will provide you with the same output. Secondly, it does not change anything outside the function.

Let's compare this to an impure implementation.

```javascript
const a = 5;
const b = 7;
let someNumber = 9;

const someNumberLogger = () => someNumber;

console.log(someNumberLogger()); // 9

const impure = () => {
    someNumber = a + b;
    return someNumber;
}

console.log(impure()); // 12
console.log(someNumberLogger()); // 12
```

`impure` function gives us the sum of two numbers, but it is not pure anymore.

* It depends on external variables a and b to produce the result. These can change at any moment resulting in a different output.
* The `impure` function stores the output in an external variable, changing it and causing side effects. When we first call `someNumberLogger` it produces 9 and then 12 because of what `impure` did.
