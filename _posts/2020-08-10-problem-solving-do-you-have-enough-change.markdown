---
layout: post
title:  "Problem Solving: Do You Have Enough Change"
date:   2020-08-10 12:00:00 -0500
---

### Problem Statement

Given a total due and an array representing the amount of change in your pocket, determine whether or not you are able to pay for the item. Change will always be represented in the following order: quarters, dimes, nickels, pennies.

To illustrate: `changeEnough([25, 20, 5, 0], 4.25)` should yield true, since having 25 quarters, 20 dimes, 5 nickels and 0 pennies gives you 6.25 + 2 + .25 + 0 = 8.50.

```javascript
changeEnough([2, 100, 0, 0], 14.11) ➞ false
changeEnough([0, 0, 20, 5], 0.75) ➞ true
changeEnough([30, 40, 20, 5], 12.55) ➞ true
changeEnough([10, 0, 0, 50], 3.85) ➞ false
changeEnough([1, 0, 5, 219], 19.99) ➞ false
```

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a solution.

### Solution

Using plain old javascript:

```javascript
const changeEnough = (changeArray, moneyNeeded) => 25 * changeArray[0] + 10 * changeArray[1] + 5 * changeArray[2] + changeArray[3] >= moneyNeeded * 100;
```

or slightly more readable:

```javascript
const changeEnough = (changeArray, moneyNeeded) => {
    const moneyInPocket = 25 * changeArray[0] + 10 * changeArray[1] + 5 * changeArray[2] + changeArray[3];
    return moneyInPocket >= moneyNeeded * 100;
}
```

or using destructuring of arrays:

```javascript
const changeEnough = (changeArray, moneyNeeded) => {
    const [quarters, dimes, nickels, pennies] = changeArray;
    const moneyInPocket = quarters * .25 + dimes * .10 + nickels * .05 + pennies;
    return moneyInPocket >= moneyNeeded;
}
```
