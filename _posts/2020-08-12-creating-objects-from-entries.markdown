---
layout: post
title:  "Creating Object From Entries"
date:   2020-08-12 12:00:00 -0500
---

We can create objects using `Object.create()`, using the curly bracket notation `{}`, with the `new` operator or ES6 Classes. ES2019 allows us to create objects from entries. Entries refer to a list of key-value pair.

```javascript
const entries = new Map([
    ["name", "Parwinder"],
    ["title", "Software Architect"],
    ["age", 97]
]);

const obj = Object.fromEntries(entries);

console.log(obj);
```

The output will be:

```javascript
{
  age: 97,
  name: "Parwinder",
  title: "Software Architect"
}
```

You do not necessarily have to use a **Map** in `fromEntries`. It works with any iterable like **Array**, **Map** or objects implementing iterables.

We can re-write the above example using **Arrays**.

```javascript
const entries = [
    ["name", "Parwinder"],
    ["title", "Software Architect"],
    ["age", 97]
];

const obj = Object.fromEntries(entries);

console.log(obj);
```

It will yield the same output.

🚨 `Object.fromEntries` is the exact opposite of `Object.entries`.

### Real-life example

Plenty of times we need the URL query params, and we all have a helper method in our code snippets, or we copy something from StackOverflow. JavaScript introduced `URLSearchParams` to make our lives easy. We can pair it with `fromEntries` to get a more readable output.

```javascript
const queryParams = "redirectFrom=login&css=blue&token=747";
const params = new URLSearchParams(queryParams);

const paramObject = Object.fromEntries(params);

console.log(paramObject); // { css: "blue", redirectFrom: "login", token: "747" }
console.log(paramObject.redirectFrom); // "login"
console.log(paramObject.css); // "blue"
console.log(paramObject.token); // "747"
```
