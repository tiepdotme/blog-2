---
layout: post
title: "Extending JavaScript Built-in Classes/Objects"
date: 2020-07-25 00:33:00 -0500
---

You might want to extend the functionality provided by JS built-in objects or classes. Maybe there is functionality on Arrays or Strings that is not yet supported by JavaScript, but you use it heavily in your project. Typically you can do this by creating a standard utility function.

Let's take the example of shuffling an array. I can create a shuffle function that takes in an array.

```javascript
function shuffle(arr) {
    for (let i = arr.length - 1; i > 0; i--) {
        const random = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[random]] = [arr[random], arr[i]];
    }
    return arr;
}

const input = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(shuffle(input)); // [ 7, 8, 10, 3, 2, 9, 5, 1, 4, 6 ]
```

**Note**: You will get a different console output compared to me as we are randomly shuffling an array.

We could, however, add this shuffle method to the prototype of Array so we can use it as in-built methods (`pop`, `slice`, `sort`).

```javascript
Array.prototype.shuffle = function () {
    let arr = this;
    for (let i = arr.length - 1; i > 0; i--) {
        const random = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[random]] = [arr[random], arr[i]];
    }
    return arr;
}

const input = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
console.log(input.shuffle()); // [ 2, 4, 9, 8, 5, 7, 3, 10, 6, 1 ]
```

Now we have declared it on the prototype, and we can call it on any array using the format `input.shuffle()`.

### Disadvantages

A lot of people do not recommend extending built-in classes or objects. It is a controversial topic. There are a couple of disadvantages:

1. EcmaScript may implement its version of the custom method we created. For example, if the next ES version brings in `shuffle` property to Arrays, we will have a conflict.
2. Nothing is stopping us from overwriting the functionality of existing methods. For example, I might change the behavior of `slice` method on Arrays with a custom function. Doing so is not a good practice for an enterprise app as others might be dependent on in-built methods. I would be breaking functionality for a lot of people in the organization.

