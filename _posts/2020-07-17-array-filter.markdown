---
layout: post
title: "Array Filter Explained"
image: "/assets/social/array-filter.png"
date: 2020-07-17 12:00:00 -0500
---

`filter()` method on an array produces a new array with all elements from the input array that pass the test. It takes a callback function with a current element, index and the original array. The last two arguments to the callback (index and array) are optional.

`filter()` is helpful in situations where you need to do some work on every element of the array and find only the elements that satisfy your criteria. Maybe you need all the even numbers from an array of numbers or only words >= six characters.

```javascript
const names = ["Parwinder", "Leah", "Lauren", "Eliu", "Robert", "George", "Eric"];
const output = names.filter(name => name.length >= 6);

console.log(output); // [ 'Parwinder', 'Lauren', 'Robert', 'George' ]
console.log(names); // [ 'Parwinder', 'Leah', 'Lauren', 'Eliu', 'Robert', 'George', 'Eric' ]
```

🚨 `filter()` does not mutate the array. The input array will stay unmodified (see example above).

You can also access the current index of the array in a `filter()`. The callback function takes a second argument for index.

```javascript
const arr = [1, 2, 4, 9, 22, 75, 16];
const filter = arr.filter((current, index) => (current % index === 0));
// return values that are divisible by the index they are on
console.log(filter); // [ 2, 4, 9, 75 ]
```

And if access to index is not enough, you can also get access to the original array as a third parameter.

🚨 `filter` has a second parameter, `this`. It is precisely like the `map` method. It specifies the `this` context for the callback function.
