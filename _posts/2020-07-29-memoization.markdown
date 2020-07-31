---
layout: post
title:  "Memoization"
date:   2020-07-29 12:00:00 -0500
---

JavaScript and functional programming go hand in hand. There are times when our functions will be expensive in terms of performance. Performing the same functions with same input over and over again could degrade the experience of the application and it is unnecessary.

**Memoization** is an optimization technique in which we cache the function results. If we provide the same input again, we fetch the result from cache instead of executing code that can cause a performance hit.

In case the result is not cached, we would execute the function and cache the result. Let's take an example of finding the square of a number.

```javascript
const square = () => {
    let cache = {}; // set cache
    return (value) => {
        if (value in cache) { // if exists in cache return from cache
            console.log("Fetching from cache");
            return cache[value];
        } else {
            // If not in cache perform operation
            console.log("Performing expensive query");
            const result = value * value;
            cache[value] = result; // store the value in cache
            return result; // return result
        }
    }
}

const sq = square();
console.log(sq(21)); // Performing expensive query, 441
console.log(sq(21)); // Fetching from cache, 441
```

### Why or when to use it?

* For expensive function calls, i.e. functions that do network calls where the output might not change or functions with large computations or disk I/O.
* For pure functions (where function output stays the same given the same input).
* For functions with limited range of input but highly recurring.
