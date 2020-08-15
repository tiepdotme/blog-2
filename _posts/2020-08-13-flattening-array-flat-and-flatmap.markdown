---
layout: post
title:  "Flattening an Array: flat & flatMap"
date:   2020-08-13 12:00:00 -0500
---

ES2019 introduced two new methods on Arrays: `flat` and `flatMap`.

### flat

The `flat` method creates a new array with all the sub-arrays concatenated or flattened. It performs this operation recursively to the specified depth.

```javascript
const arr = [0, 1, 2, [3, 4], [5, [6, 7]]];
console.log(arr.flat()); // [ 0, 1, 2, 3, 4, 5, [ 6, 7 ] ]
```

Well, it's still not flat! I can see that `[6, 7]` exist in the resulting array. This is where the depth property comes in. If no depth is specified, `flat` goes one level deep.

```javascript
const arr = [0, 1, 2, [3, 4], [5, [6, 7]]];
console.log(arr.flat(2)); // [ 0, 1, 2, 3, 4, 5, 6, 7 ]
```

`flat` will also help you to flatten arrays with "holes".

```javascript
const arr = [1, 2, , 4, 5, , 7];
arr5.flat(); // [1, 2, 4, 5, 7]
```

🚨 Use arrays having empty indices with caution, not all browsers support them.

### flatMap

`flatMap` is a combination of `map` and `flat` (as the name suggests). `flatMap` takes every element of an iterable and runs it through a mapping function. Then it flattens the result into a new array.

It takes two arguments:
- a callback and
- `this` context

The callback can be passed three parameters:
- current element being processed
- index and
- array

The callback function with the current element being processed is required. Everything else is optional.

```javascript
const arr = [1, 3, 5].flatMap(x => [x, x + 1]);
console.log(arr); // [ 1, 2, 3, 4, 5, 6 ]
```

What is happening in the above example?

- We take an input array with 1, 3, 5 as elements
- For each value, we run a mapping function that returns an array of the value and the value incremented by 1
- Example, for value 1 we get an array of 1 and 2
- Once we have three arrays for three elements, `flatMap` flattens it
- We get the final array

Hope the short blog post helped you understand the concept of `flat` and `flatMap`.

Happy coding 👋🏼
