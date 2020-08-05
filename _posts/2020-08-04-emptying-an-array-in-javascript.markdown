---
layout: post
title:  "How to Empty an Array in JavaScript"
date:   2020-08-04 12:00:00 -0500
---

I ask this question fairly often in my interviews. The problem is so simple that a lot of candidates get thrown off by it as if it is a trick question. The reason I ask is that JavaScript provides an insane amount of ways to do this trivial task.

* The easiest of them all is reassigning our array variable to an empty array

```javascript
let arr = [1, 2, 4, "Joe"];
arr = [];
console.log(arr); // []
```
The code above will set the variable `arr` to a new empty array. It is recommended if you don't have **references to the original array** `arr` anywhere else because it will create a new empty array. If you have referenced this array from another variable, then the original reference array will remain unchanged. Let me illustrate the problem with this solution.

```javascript
let arr = ['a', 'b', 'c', 'd', 'e', 'f'];
let anotherArr = arr;
arr = [];
console.log(anotherArr); // ['a', 'b', 'c', 'd', 'e', 'f']
```

* Using the length property

```javascript
let arr = [1, 2, 4, "Joe"];
arr.length = 0;
console.log(arr); // []
```

The code above will clear the existing array by setting its length to 0. This way of emptying an array will also update all the reference variables that point to the original array.

* Using the splice method

```javascript
let arr = [1, 2, 4, "Joe"];
arr.splice(0, arr.length);
console.log(arr); // []
```

Above implementation will also work correctly. This way of empty the array will also update all the references of the original array.

* Using `while` loop

```javascript
let arr = [1, 2, 4, "Joe"];
while (arr.length) {
    arr.pop();
}
console.log(arr); // []
```

* Using `while` with `shift`

```javascript
let arr = [1, 2, 4, "Joe"];
while (arr.length) {
    arr.shift();
}
console.log(arr); // []
```

If you do have any other ways, please share.
