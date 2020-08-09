---
layout: post
title:  "Problem Solving: Check if a Value is an Array"
date:   2020-08-09 12:00:00 -0500
---

### Problem Statement

Given an input value, check if it is an Array.

We often encounter situations where we need to know whether the value is an array or not. For instance, the code below performs some operation based value type.

```javascript
function myFunc(value) {
    if ("value is an array") {
        // do something
    } else {
        // otherwise
    }
}
```

Let's discuss some way to detect an array in JavaScript.

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a solution.

### Solution


**Method 1:**

Juriy Zaytsev (Also known as kangax) proposed an elegant solution to this.

```javascript
const isArray = (value) => {
    return Object.prototype.toString.call(value) === '[object Array]';
}

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false
```

Above approach relies on the fact that native `toString()` method on a given value produces a standard string in all browser. It works effectively in older browsers.

**Method 2:**

Duck typing test for array type detection

```javascript
const isArray = (value) => {
    return typeof value.sort === 'function';
}

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false
```

As we can see above the isArray method will return true if the `value` object has `sort` method of type `function`. Now assume you have created an object with the `sort` method.

```javascript
const isArray = (value) => {
    return typeof value.sort === 'function';
}

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false

const bar = {
    sort: function () { }
}

console.log(isArray(bar)); // true
```

Now when you check `isArray(bar)` then it will return true because `bar` object has sort method, But the fact is `bar` is not an array. It's not the ideal way of detecting an array.

**Method 3:**

ECMAScript 5 has introduced **Array.isArray()** method to detect an array type value. The sole purpose of this method is accurately detecting whether a value is an array or not. We can use that method as is or pair it with our first method to make a solution with fallback.

```javascript
const isArray = (value) => {
    if (typeof Array.isArray === "function") {
        return Array.isArray(value);
    } else {
        return Object.prototype.toString.call(value) === '[object Array]';
    }
}

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false
```

**Method 4:**

Using the constructor name.

```javascript
const isArray = value => value.constructor.name === "Array";

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false
```

**Method 5:**

Using `instanceof`.

```javascript
const isArray = value => value instanceof Array;

console.log(isArray([1, 2, 3])); // true
console.log(isArray(1)); // false
```
