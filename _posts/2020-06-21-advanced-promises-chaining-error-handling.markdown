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

There are two ways in which you can handle errors in your promise chain, either by passing a error handler to `then` block or using the `catch` operator. We discussed the first method in the previous blog post.

```javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject("an error has occurred");
    }, 2000)
});

myPromise.then((response) => {
    console.log(response);
}, (error) => {
    console.log(error); // an error has occurred
});
```

In the above example, `then` has two callbacks. The first one is a success handler and the second is an error handler. This is completely fine and works for most cases. It has certain drawbacks:

1. If the success handler ends in an error, you will not catch/handle it!
2. If you are using a chain of promises like the one in the chaining example, you will be writing error handler for each `then` block.

To come over these drawbacks, we use the `catch` operator.

```javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject("an error has occurred");
    }, 2000)
});

myPromise.then((response) => {
    console.log(response);
}).catch((error) => {
    console.log(error); // an error has occured
});
```

For the chain of promises we can use a `catch` operator like:

```javascript
const waitForMe = function (name) {
    return new Promise((resolve, reject) => {
        if (name === "Robert") {
            return reject("Robert is always on time");
        } else {
            setTimeout(() => {
                return resolve(name);
            }, 2000);
        }
    });
}

waitForMe("Parwinder")
    .then((data) => {
        console.log(data); // wait 2 second and log "Parwinder"
        return waitForMe("Lauren");
    })
    .then((data) => {
        console.log(data); // wait 2 more seconds and log "Lauren"
        return waitForMe("Robert"); // this will result in promise rejection
    })
    .then((data) => {
        console.log(data); // this never gets executed
        return waitForMe("Eliu");
    })
    .then((data) => {
        console.log(data); // this never gets executed
    })
    .catch((error) => {
        console.log(error); // Robert is always on time
    })
```

Keep in mind that when chaining promises and one of the promise gets rejected, it will terminate the rest of the chain. That is why the last two console logs never run.

`catch` operator does not always have to be at the very end. It could be in the middle of the chain and it will catch the errors in the chain so far.

```javascript
const waitForMe = function (name) {
    return new Promise((resolve, reject) => {
        if (name === "Robert") {
            return reject("Robert is always on time");
        } else {
            setTimeout(() => {
                return resolve(name);
            }, 2000);
        }
    });
}

waitForMe("Parwinder")
    .then((data) => {
        console.log(data); // wait 2 second and log "Parwinder"
        return waitForMe("Lauren");
    })
    .then((data) => {
        console.log(data); // wait 2 more seconds and log "Lauren"
        return waitForMe("Robert"); // this will result in promise rejection
    })
    .catch((error) => { // catches the promise rejection
        console.log(error); // Robert is always on time
        return waitForMe("Eliu"); // continues the chain
    })
    .then((data) => {
        console.log(data); // Eliu
    })
```

🚨 Why not use `catch` all the time and ignore the error handler in `then`?

I mentioned this disadvantage above for error handler in `then`:

> If you are using a chain of promises like the one in the chaining example, you will be writing error handler for each `then` block.

There will be times when you **DO** want different error handlers for all `then` blocks in your chain (maybe for easier debugging or logging). At that point, error handler in individual `then` blocks become an advantage.

### Operators

There are two key operators that promises have which are suited for specific conditions: `Promise.all` and `Promise.race`.

#### Promise.all

#### Promise.race
