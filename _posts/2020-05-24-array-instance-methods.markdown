---
layout: post
title:  "Array (Instance) Methods v2"
date:   2020-05-24 16:45:00 -0500
---

## Array Instance Methods

Methods that exist on the prototype of `Array`

### Concat

Returns and array joined (concatenated) with another array or values.

```javascript
const array1 = ["a", "b", "c"];
const array2 = ["d", "e", "f"];
const array3 = array1.concat(array2);

console.log(array3); // ["a", "b", "c", "d", "e", "f"]
```

You can also concatenate values to an array.

```javascript
const letters = ["a", "b", "c"];
const alphaNumeric = letters.concat(1, [2, 3]);
console.log(alphaNumeric); // ["a", "b", "c", 1, 2, 3]
```

Or concatenate nested arrays.

```javascript
const num1 = [[1]];
const num2 = [2, [3]];
const numbers = num1.concat(num2);

console.log(numbers); // [[1], 2, [3]]

// modify the first element of num1
num1[0].push(4);
console.log(numbers); // [[1, 4], 2, [3]]
```

### Entries

It is relatively common to use the method `entries` or `keys` or `values` on an object, but they are also supported on arrays.

`Entries` method returns an **iterator** with key/value pair.

```javascript
const array1 = ["a", "b", "c"];
const iterator = array1.entries();

console.log(iterator.next().value); // [0, "a"]
console.log(iterator.next().value); // [1, "b"]
```

### Keys

`Keys` method returns an **iterator** with keys.

```javascript
const array1 = ["a", "b", "c"];
const iterator = array1.keys();

console.log(iterator.next().value); // 0
console.log(iterator.next().value); // 1
console.log(iterator.next().value); // 2
```

### Values

```javascript
const array2 = ["a", "b", "c"];
const i = array2.values();

console.log(i.next().value); // a
console.log(i.next().value); // b
console.log(i.next().value); // c
```

### Includes

`Includes` method checks if an array contains an element and returns true or false.

```javascript
const array1 = [1, 2, 3];
console.log(array1.includes(3)); // true
console.log(array1.includes(4)); // false

const pets = ["cat", "dog", "bat"];
console.log(pets.includes("cat")); // true
console.log(pets.includes("at")); // false
```

The includes method also takes **index** as the second parameter. The second parameter makes the include method check for a value in an array with an index greater than or equal to the provided index.

```javascript
let example = ["a", "b", "c"]

example.includes("b", 3); // false
example.includes("b", 100); // false
example.includes("b", 1); // true
```

### indexOf

`indexOf` method returns the first index of the given element if it is present in the array. If it is not, it returns -1. Folks used it to check if an element exists in an array before ES6. There is no specific need to use indexOf when includes exist.

Use the includes() method to check if the element is present in the array. If you need to know where the element is in the array, you need to use the indexOf() method.

```javascript
var array = [2, 9, 9];
array.indexOf(2); // 0
array.indexOf(7); // -1
array.indexOf(9, 2); // 2
array.indexOf(2, -1); // -1
array.indexOf(2, -3); // 0
```

### findIndex

At this point, you have learned about `indexOf` and `includes` to find element or index. `findIndex` is somewhat similar. `findIndex` provides you the index of the first element that satisfies a callback or a testing function.

indexOf expects the value you are looking for as a parameter. findIndex looks for a callback or testing function as its parameter. I would suggest using indexOf in arrays with primitive types like strings, numbers, or booleans. Use findIndex when dealing with non-primitive types like objects, and your find condition is relatively complicated.

```javascript
const fruits = ["apple", "banana", "cantaloupe", "blueberries", "grapefruit"];
const index = fruits.findIndex(fruit => fruit === "blueberries");

console.log(index); // 3
console.log(fruits[index]); // blueberries
```

### Find

I believe you think I am trolling you with all these methods that `find` if the index or value exists in an array. They all have a very subtle difference, and I have listed the differences in each method description.

The `find` method returns the **value** of the first element that matches the callback or the testing condition. `find` gets you value and `findIndex` gets you index. 🙂

```javascript
const array = [7, 33, 47, 99, 2, 103, 79];
const found = array.find(element => element > 10);
console.log(found); // 33
```

### Join

`join` method is a relatively common and frequently used method. It creates and returns a string by concatenating all of the elements in an array. You can join all elements or provide a separator to connect them with. By default, the method uses a comma `(,)` as a separator.

```javascript
const fruits = ["Apple", "Banana", "Raspberry"];

console.log(fruits.join()); // Apple,Banana,Raspberry
console.log(fruits.join("")); // AppleBananaRaspberry
console.log(fruits.join("|")); // Apple|Banana|Raspberry
```
