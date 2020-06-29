---
layout: post
title:  "Array (Instance) Methods Part II"
image: "/assets/social/array-instance-methods-ii.png"
date:   2020-05-25 13:45:00 -0500
---

## Array Instance Methods

Methods that exist on the prototype of `Array`. This is the third post in the series of array methods. You can read the first post about [array static methods here](/blog/2020/05/23/array-methods-v1.html) and the [second post here](/blog/2020/05/24/array-instance-methods.html) (part 1 of instance methods).

### lastIndexOf
lastIndexOf gets you the last index at which a given element can be found. If not found, it returns -1. It can take a second parameter that is used as `fromIndex`. The lookup starts **backward** from the index provided.

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

The pop method removes the last element of the array and returns the removed element (not the array).

```javascript
const numbers = [1, 22, 123, 0, 15, 9, 88, 123, 0, 45];
console.log(numbers.pop()); // 45
console.log(numbers.pop()); // 0
console.log(numbers.pop()); // 123
console.log(numbers.pop()); // 88
```

### reverse
The reverse method reverses the array in place. It returns the reversed array.

🚨 This is a mutable method. It changes the original array (similar to sort).

```javascript
const original = ['one', 'two', 'three'];
const reversed = original.reverse();
console.log(reversed); // ["three", "two", "one"]
console.log(original); // ["three", "two", "one"]
```

### shift
The shift method is similar to pop. Shift removes an element from the beginning, whereas pop removes the element from the end of the array. Both methods return the removed element.

```javascript
const original = [1, 2, 3]
const firstRemoved = original.shift();
console.log(original); // [2, 3]
console.log(firstRemoved); // 1
```

### some
Some array method checks if at least one element in the array meets the callback or testing function.

```javascript
const array = [1, 2, 3, 4, 5];
const even = (element) => element % 2 === 0; // checks whether an element is even
const greaterThanTen = (element) => element > 10; // checks whether an element is greater than 10
console.log(array.some(even)); // true
console.log(array.some(greaterThanTen)); // false
```

### sort
The sort method sorts the array in ascending (**by first converting into string**) order in place.

```javascript
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months); // ["Dec", "Feb", "Jan", "March"]

const numbers = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1); // [1, 100000, 21, 30, 4]
```

Notice how 100000 was before 21 in the last example. That happens because of conversion to a string before sorting.

The sort method allows you to do a custom sort by providing a compare function with two elements for comparison.

```javascript
const numbers = [1, 30, 4, 21, 100000];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers); // [ 1, 4, 21, 30, 100000 ]
```

This example provides you the number sort you might have been looking for 🙂

### toString
toString returns a string from array elements

```javascript
const sample = [1, 2, "Parwinder", true, "Hello", 78];
console.log(sample.toString()); // 1,2,Parwinder,true,Hello,78
```

### unshift
The unshift method is similar to the push method. The push method adds an element to the end of an array. Unshift adds an element to the beginning of an array. Both return the length on the new array. **Neither returns the new array**.

```javascript
const numbers = [1, 2, 3];
console.log(numbers.unshift(4, 5, 6)); // 6
console.log(numbers); // [ 4, 5, 6, 1, 2, 3 ]
```

That would be all about arrays for now. There are a few uncommon methods that I am missing. In case you are interested check out the [array methods page from MDN here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
