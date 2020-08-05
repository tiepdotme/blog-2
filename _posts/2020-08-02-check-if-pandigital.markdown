---
layout: post
title:  "Problem Solving: Check if a number is pandigital"
date:   2020-08-02 12:00:00 -0500
---

With this blog post, I am starting a new series that focuses on problem-solving. For some problems we might be looking for the most efficient solutions, for others, we might want the most bizarre and convoluted way of solving the problem. We might come up with the most "clever" solution or something that works! The challenge is not only the problem statement but how we would like to achieve it.

### Problem Statement

A pandigital number contains all digits (0-9) at least once. Write a function that takes an integer, returning true if the integer is pandigital, and false otherwise. It is safe to assume that the input with always is an integer.

**We are looking for a clever solution.**

#### Examples

```javascript
isPandigital(98140723568910) ➞ true
isPandigital(90864523148909) ➞ false // 7 missing
isPandigital(1223344556677889) ➞ false // 0 missing
```

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a hint, and the section after that gives you the solution.

#### Hint

Think about the properties of a pandigital number when all duplicates are removed 🤔

### Solution

```javascript
const isPandigital = num => new Set(num.toString().split('')).size === 10;
```

One of the critical properties of `Sets` is that all elements are distinct. When we remove duplicates from our input using a `Set`, and we are looking for a pandigital number, the result should have a length of 10 (numbers from 0 to 9).

Just because I was looking for a smart solution does not mean it can't be solved in any other way. There are chances that a person might not know about `Sets` or they might not be aware of the implementation of them in JavaScript. Here are a couple more ways to solve this problem.

```javascript
const isPandigital = num => /0+1+2+3+4+5+6+7+8+9+/.test(String(num).split('').sort().join(''));
```

**OR using sets in a more complicated manner 😉**

```javascript
const isPandigital = num => {
    let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
    let sequence = [...new Set(String(num).split(''))]
        .sort((a, b) => a - b)
        .map(x => Number(x));

    return sequence.join('') === numbers.join('');
}
```
