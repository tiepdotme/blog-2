---
layout: post
title:  "The Empty Statement in JavaScript"
date:   2020-08-15 12:00:00 -0500
---

The empty statement in JavaScript is one of those fun and quirky things about JS that you should know. It might not be beneficial, but it exists, and it is entirely legal. An empty statement in JavaScript is `;`. Yup, a semicolon.

An empty statement provides no statement even though JavaScript expects it. The statement has no effect and performs no action.

A typical example would be to create a for loop that has no body.

```javascript
const arr = [1, 2, 3, 4, 5];

for (i = 0; i < arr.length; arr[i++] = 0) ;

console.log(arr); // [ 0, 0, 0, 0, 0 ]
```

It is a good idea to leave a comment if you ever plan on using an empty statement.

```javascript
const arr = [1, 2, 3, 4, 5];

for (i = 0; i < arr.length; arr[i++] = 0) /* empty */ ;

console.log(arr); // [ 0, 0, 0, 0, 0 ]
```

Another example of using an empty statement is a chain of `if-else`.

```javascript
const name = "Lauren";

if (name === "Parwinder")
    console.log(name);
else if (name === "Lauren")
    console.log(`Hello ${name}`); // Hello Lauren
else if (name === "Eliu"); // No action is taken if name passed is  "Eliu"
else if (name === "Robert")
    console.log(`Good to see you ${name}`);
else
    console.log("Goodbye");
```
