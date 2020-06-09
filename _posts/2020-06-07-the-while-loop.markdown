---
layout: post
title:  "While Loop"
date:   2020-06-07 12:00:00 -0500
---

## Introduction

Loops allow us to repeat the same action multiple times. Every loop has three key items:

1. Loop start
2. Loop end
3. Loop increment/decrement/counter

For example, we might want to log numbers from 1 to 10. Here, the start is 1, the end is 10, and the counter increments by 1 every time.

```javascript
let i = 1; // start
while (i <= 10) { // end
  console.log(i); // 1 2 3 4 5 6 7 8 9 10
  i++; // increment/counter
}
```

🚨If we did not have the increment or counter, the loop will go on forever and log `1` infinite times.

The expression that is evaluated for the end of the loop need not be a comparison. Any falsy expression or variable will end the loop.

```javascript
let i = 10; // start
while (i) { // end
  console.log(i); // 10 9 8 7 6 5 4 3 2 1
  i--; // decrement/counter
}
```

When `i` reaches 0, it is falsy, and the loop will end.

The example above could also be turned into a one-line while loop. We can also omit braces when it is a one-liner loop.

```javascript
let i = 10;
while (i) console.log(i--);
```
