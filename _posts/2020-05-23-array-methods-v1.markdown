---
layout: post
title:  "Array Methods"
image: "/assets/social/array-methods.png"
date:   2020-05-23 16:45:00 -0500
---

## Array Utility Methods

Methods that exist on the parent `Array`

### Array.of

```javascript
const x = Array.of("Parwinder", "Bhagat");
console.log(x); // [ 'Parwinder', 'Bhagat' ] 

// Example with the spread operator

const y = Array.of(..."Parwinder");
console.log(y); // [ 'P', 'a', 'r', 'w', 'i', 'n', 'd', 'e', 'r' ]
```

### Array.from

Creates an array with the given length, items, array-like or iterable object.

```javascript
const x = Array.from({ length: 3 });
console.log(x); // [ undefined, undefined, undefined ]

// With items using a callback function as second parameter
const y = Array.from({ length: 3 }, function () {
    return "Hello";
});
console.log(y); // [ 'Hello', 'Hello', 'Hello' ]


// With items using a callback function and arguments
const y = Array.from({ length: 3 }, function (value, index) {
    return index;
});
console.log(y); // [ 0, 1, 2 ]

```

### Array.isArray()

`typeof` an array returns `object` as a type, so this is where isArray comes in handy.

```javascript
const x = ["Hello", "World"];
console.log(Array.isArray(x)); // true

const y = 27;
console.log(Array.isArray(y)); // false
```


