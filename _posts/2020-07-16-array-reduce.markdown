---
layout: post
title: "Array Reduce"
date: 2020-07-16 12:00:00 -0500
---

`reduce()` method on an array produces a single value by applying a reducer function to every element of the input array. It takes an input array, an accumulator (it is maintained throughout the loop), current value and index.

`reduce()` is helpful in situations where you need to do some work on every element of the array and produce one result. Maybe you need to add every element of an array.

```javascript
const arr = [2, 4, 9, 22];
const reduce = arr.reduce((final, current) => final + current);
console.log(reduce); // 37 (2 + 4 + 9 + 22)
```

Here `final` is the accumulator in which we keep adding all the values of the original input array. `current` denotes the current value of the loop.

You can also access the current index of the array using a map. The callback function takes a second argument for index.

```javascript
const arr = [2, 4, 9, 22];
// we will add all numbers as well as their indices
const reduce = arr.reduce((final, current, index) => final + current + index);
console.log(reduce); // 43 (2 + 0 + 4 + 1 + 9 + 2 + 22 + 3)
```

And if access to index is not enough, you can also get access to the original array as a fourth parameter.

🚨 Unlike `map()`, `reduce()` does not change the original array.

We will go over one more example to understand the power of `reduce` and the situations it could be used in. We will flatten an array without the use of `flat` method.

```javascript
const flatArray = (arr) => {
    let output = [];
    return arr.reduce((final, value) => {
        // use recursion, if value then concat to our final array else call flatArray again with the child array
        return final.concat(Array.isArray(value) ? flatArray(value) : value);
    }, output);
}

const input = [1, 2, 3, [10, 11, 12], 21, 22, 23, [31, 32, 33, 34], [41, 42]];
console.log(flatArray(input)); // [ 1, 2, 3, 10, 11, 12, 21, 22, 23, 31, 32, 33, 34, 41, 42 ]
```

🚨 `reduce` has a second parameter, and it is not the `this` context like a `map`; instead, it is the initial value to start with!

Look at the example above and see that we have provided the initial value as `output`. If no value is provided, it takes the first element of the array as the initial value, technically starting the loop from the second value of an array.
