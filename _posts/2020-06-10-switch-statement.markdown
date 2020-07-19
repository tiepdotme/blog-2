---
layout: post
title:  "Switch Statement"
image: "/assets/social/switch.png"
date:   2020-06-10 12:00:00 -0500
---

## Introduction

`switch` is a conditional statement that will evaluate an expression and execute multiple statements based on the value of the expression.

Think of it as a multi if statement.

## Key Parts

1. Expression to evaluate
2. Case blocks
3. (Optional) Default block

## Syntax

```javascript
switch (expressopm) {
    case value1:
        //Statements executed when the
        //result of expression matches value1
        break; // break from further evaluation
    case value2:
        //Statements executed when the
        //result of expression matches value2
        break;
    case valueN:
        //Statements executed when the
        //result of expression matches valueN
        break;
    default:
        //Statements executed when none of
        //the values match the value of the expression
        break;
}
```

## Example

```javascript
const tellMeTheNumber = (num) => {
    switch(num) {
        case 1:
            console.log("You are number one!");
            break;
        case 2:
            console.log("Second is not a bad place to be.");
            break;
        case 3:
            console.log("Three Three Three");
            break;
        case 4:
            console.log("Quad");
            break;
        default:
            console.log("I don't know who I am anymore?");
            break;
    }
}

tellMeTheNumber(4); // Quad
tellMeTheNumber(1); // You are number one!
tellMeTheNumber(1); // I don't know who I am anymore?
```

## Missing a `break`?

🚨If we miss a break statement in any **case** within a switch statement, all the following cases will execute without meeting the criteria.

```javascript
const tellMeTheNumber = (num) => {
    switch(num) {
        case 1:
            console.log("You are number one!");
        case 2:
            console.log("Second is not a bad place to be.");
        case 3:
            console.log("Three Three Three");
        case 4:
            console.log("Quad");
        default:
            console.log("I don't know who I am anymore?");
            break;
    }
}

tellMeTheNumber(1);
// You are number one!
// Second is not a bad place to be.
// Three Three Three
// Quad
// I don't know who I am anymore?
```

We asked for case `1` in the above example, and all the cases were missing `break` statement. It will continue through cases 2, 3, 4, and default without meeting the criteria `1`.

## Grouping cases

If there are multiple cases in a `switch` statement, we might want to execute the same action for a subset of these cases. To avoid code duplication, we can group such cases.

```javascript
const tellMeTheNumber = (num) => {
    switch (num) {
        case 1:
        case 2:
        case 3:
            console.log("You are in top 3");
            break;
        case 4:
            console.log("You did not make it this time");
            break;
        default:
            console.log("I don't know who I am anymore?");
            break;
    }
}

tellMeTheNumber(2); // You are in top 3
tellMeTheNumber(4); // You did not make it this time
tellMeTheNumber(12); // I don't know who I am anymore?
```

Number 1, 2, or 3 will generate the same message.

## Strict type check

The expression evaluated by a switch case statement uses `===` for equality of value and type. So if we pass the string `"3"` vs. number 3, you have different results.

```javascript
const tellMeTheNumber = (num) => {
    switch (num) {
        case 1:
        case 2:
        case 3:
            console.log("You are in top 3");
            break;
        case 4:
            console.log("You did not make it this time");
            break;
        default:
            console.log("I don't know who I am anymore?");
            break;
    }
}

tellMeTheNumber(3); // You are in top 3
tellMeTheNumber("3"); // I don't know who I am anymore?
```

Since the string `"3"` did not match any case, the `default` case was executed.

## Block scoping of cases

ES6 or ES2015 allows the use of `let` and `const` to create block scope. If we use them in a `switch` statement, keep in mind that the block is at the level of `switch` statement and not at the case level.

To have blocks at the case level, wrap the case clauses in brackets.

```javascript
const tellMeTheNumber = (num) => {
    switch (num) {
        case 1:
        case 2:
        case 3: {
            let message = "You are in top 3";
            console.log(message);
            break;
        }
        case 4: {
            let message = "You did not make it this time";
            console.log(message);
            break;
        }
        default: {
            let message = "I don't know who I am anymore?";
            console.log(message);
            break;
        }
    }
}

tellMeTheNumber(2); // You are in top 3
tellMeTheNumber(4); // You did not make it this time
tellMeTheNumber(12); // I don't know who I am anymore?
```
