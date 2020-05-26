---
layout: post
title:  "Array (Instance) Methods Part II"
date:   2020-05-23 16:45:00 -0500
---

## Array Instance Methods

Methods that exist on the prototype of `Array`. This is the third post in the series of array methods. You can read the first post about [array static methods here](2020-05-23-array-methods-v1.markdown) and the [second post here](2020-05-24-array-instance-methods.markdown) (part 1 of instance methods).

### lastIndexOf
lastIndexOf gets you the last index at which a given element can be found. If not found it returns -1. It can take a second parameter that is use as `fromIndex`. The lookup starts **backwards** from the index provided.

```javascript
const numbers = [1, 22, 123, 0, 15, 9, 88, 123, 0, 45];
console.log(numbers.lastIndexOf(2)); // -1
console.log(numbers.lastIndexOf(22)); // 1
console.log(numbers.lastIndexOf(0)); // 8
console.log(numbers.lastIndexOf(123, 4)); // 2
console.log(numbers.lastIndexOf(0, 6)); // 3
console.log(numbers.lastIndexOf(1, 1)); // 0
```

### pop

### reverse

### shift

### some

### sort

### toString

### unshift

That would be all about arrays for now. There are a few uncommon methods that I am missing. In case you are interested checkout the [array methods page from MDN here]()
