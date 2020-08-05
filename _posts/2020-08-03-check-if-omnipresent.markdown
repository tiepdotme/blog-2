---
layout: post
title:  "Problem Solving: Check if a value is omnipresent"
date:   2020-08-03 12:00:00 -0500
---

### Problem Statement

A value is omnipresent if it exists in every subarray inside the main array. Create a function that determines whether an input value is omnipresent for a given array. Sub-arrays can be any length.

To illustrate:
```javascript
[[3, 4], [8, 3, 2], [3], [9, 3], [5, 3], [4, 3]]
// 3 exists in every element inside this array, so is omnipresent.
```

**We are looking for a short solution.**

#### Examples

```javascript
isOmnipresent([[1, 1], [1, 3], [5, 1], [6, 1]], 1) ➞ true
isOmnipresent([[1, 1], [1, 3], [5, 1], [6, 1]], 6) ➞ false
isOmnipresent([[5], [5], [5], [6, 5]], 5) ➞ true
isOmnipresent([[5], [5], [5], [6, 5]], 6) ➞ false
```

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a solution.

### Solution

```javascript
const isOmnipresent = (arr, val) => arr.every(x => x.includes(val));
```

The `every()` method tests whether all elements in the array pass the test implemented by the provided function. In this case, our function checks for the inclusion of value in every subarray using `includes`.

Here is another way to solve it:

```javascript
function isOmnipresent(arr, val) {
    for (let i = 0; i < arr.length; i++) {
        if (!arr[i].includes(val)) {
            return false
        }
    }
    return true;
}
```
