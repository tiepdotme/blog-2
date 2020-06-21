---
layout: post
title: "Advanced Promises: Chaining, Error Handling & Operators"
date: 2020-06-21 12:00:00 -0500
---

The last [blog post](https://bhagat.me/blog/2020/06/20/basics-of-promises.html) details about what promises are, how to create them, how do they resolve and how we can reject them.

This time we will be going over chaining in promises along with error handling and operators available.

### Chaining

One of the biggest drawback of callbacks was the nested structure they formed when we would chain them. With the help of `then` operator we can create a flat structure that is easier to read, understand and debug.

Lets say we have a function `waitForMe` that returns a promise. This function waits two second for a friend of your and then yells (outputs in console) their name.

```javascript
const waitForMe = function(name) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            return resolve(name);
        }, 2000);
    });
}

waitForMe("Parwinder")
    .then((data) => {
        console.log(data); // Outputs/yells "Parwinder" after 2 second
    });
```

You have a lot of lazy friends and you would like to call them all as you are in a hurry. We will call them one by one (chaining the action).

```javascript
waitForMe("Parwinder")
    .then((data) => {
        console.log(data); // waits 2 seconds and outputs "Parwinder"
        return waitForMe("Lauren");
    })
    .then((data) => {
        console.log(data); // waits another 2 seconds and outputs "Lauren"
        return waitForMe("Robert");
    })
    .then((data) => {
        console.log(data); // waits another 2 seconds and outputs "Robert"
        return waitForMe("Eliu");
    })
    .then((data) => {
        console.log(data); // waits another 2 seconds and outputs "Eliu"
    })
```

You can see how this chains calling names with two second breaks in between each console log. Every `then` operator return a promise that is further chained with another `then` while still maintaining a flat code structure.

### Error Handling

