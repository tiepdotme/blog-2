---
layout: post
title:  "eval Function"
date:   2020-08-17 12:00:00 -0500
---

`eval()` is a global function that executes JavaScript code passed as a string. It can evaluate an expression. If the arguments passed is one or more JavaScript statements, it will run them.

```javascript
console.log(eval("5 + 7")); // 12
console.log(eval("5 + 7") === eval("12")); // true

let num = 5;
eval("num = 10");
console.log(num); // 10
```

`eval()` can also call other functions.

```javascript
const multiply = (a, b) => a * b;
console.log(eval("multiply(7, 5)")); // 35
```

`eval()` is capable of converting a string into JSON.

```javascript
const input = '({"firstName" : "Parwinder", "lastName" : "Bhagat"})';
const parsedObject = eval(input);
console.log(parsedObject.firstName); // Parwinder
```

🚨Now comes the important part, do not use `eval()`!

There are two significant reasons not to use `eval()`:

- Malicious code: if you use eval with non sanitized user input, it could open up an exploit. Anyone can execute malicious code.
- Non-performant: JavaScript is capable of using various types (numbers, arrays, objects) and working solely with strings could cause performance lag.
