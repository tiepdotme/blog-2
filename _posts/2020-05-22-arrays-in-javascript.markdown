---
layout: post
title:  "Arrays in JavaScript"
date:   2020-05-22 16:45:00 -0500
---

## What are Arrays?

Array in JavaScript is a type that is used to store multiple items or a list of items where the order matters. Keep in mind that array is of `typeof` object 🤷🏽‍♂️

Every item in the array has an index. The index is the position of the element in the array. Arrays have a 0 based index. The first element in the array has the index 0, the second element in the array has the index 1 and so on.

The total number of items in the array is the length of the array.

### How can you create an array?

```javascript
// Using the square bracket notation
const carArray = ["Honda", "BMW", "Ferrari", 27, true];

// Using new keyword
const bikeArray = new Array("Honda", "Ducati", "Yamaha");
```

🚨 Warning: It is not recommended to use the **new** keyword to create an array. See below.

### How to know if a variable is an array?

Since the `typeof` array is object, when you do the following:

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
console.log(typeof(carArray)); // output will be object!
```

instead you need to do

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
console.log(Array.isArray(carArray)); // true
```

We can also do

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
carArray instanceof Array; // true
```

### How do I access the properties of an array when they don't have keys?

Use the index!

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
console.log(carArray[2]); // Ferrari
```

Keep in mind that even though Ferrari is the 3rd element in the array, it is referred to by index 2 because arrays are 0 based index.

### Changing the elements in an array
You can set the values in an array the same way as you retrieve the values. Both actions use an index.

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
carArray[2] = "Mazda";
console.log(carArray); // [ 'Honda', 'BMW', 'Mazda', 27, true ] 
```

### How many items are in an array?

Use the `length` property!

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27, true];
console.log(carArray.length); // 5
```

And if you need to access the last element of an array, you can do:

```javascript
const carArray = ["Honda", "BMW", "Ferrari", 27];
console.log(carArray[carArray.length - 1]); // 27
```

### Common array methods

We will discuss a ton of methods in the next few blog posts as we go in-depth with arrays. For now, there are two common methods I would like to discuss:

1. Push: This allows you to add an element at the end of the array
   ```javascript
   const carArray = ["Honda", "BMW", "Ferrari", 27, true];
   carArray.push("Mazda");
   console.log(carArray); // [ 'Honda', 'BMW', 'Ferrari', 27, true, 'Mazda' ] 
   ```
2. Sort:
   This sorts the array in place. It is a mutable method. It will change the original array!
   ```javascript
   const carArray = ["Honda", "BMW", "Ferrari", 27, true];
   carArray.sort();
   console.log(x); // [ 27, 'BMW', 'Ferrari', 'Honda', true ] 
   ```

### Why not to use the new keyword when creating an array?

The square bracket `[]` notation and the `new` keyword do the same thing by creating an empty array or create an array with the passed value.

The `new` keyword however, has unexpected results at times.

```javascript
let score = new Array(5, 10);
console.log(score); // [ 5, 10 ]

score = new Array(5);
console.log(score); // [ , , , ,  ]
```

The first example creates an array with items 5 and 10. The second example however, creates an array with 5 undefined elements instead of an array with element 5!

