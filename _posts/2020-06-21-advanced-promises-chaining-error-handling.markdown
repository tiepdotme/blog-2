---
layout: post
title: "Advanced Promises: Chaining, Error Handling & Operators"
image: "/assets/social/promises-advanced.png"
date: 2020-06-21 12:00:00 -0500
---

The last [blog post](https://bhagat.me/blog/2020/06/20/basics-of-promises.html) details about what promises are, how to create, how do they resolve, and how we can reject them.

This time we will be going over chaining in promises along with error handling and operators available.

### Chaining

One of the most significant drawbacks of callbacks was the nested structure they formed when we would chain them. With the `then` operator's help, we can create a flat structure that is easier to read, understand, and debug.

Let's say we have a function `waitForMe` that returns a promise. This function waits two seconds for a friend of yours and then yells (outputs in the console) their name.

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

You have a lot of lazy friends, and you would like to call them all as you are in a hurry. We will call them one by one (chaining the action).

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

You can see how we chained calling names with two-second breaks in between each console log. Every `then` operator returns a promise that is further chained with another `then` while maintaining a flat code structure.

### Error Handling

There are two ways in which you can handle errors in your promise chain, either by passing an error handler to `then` block or using the `catch` operator. We discussed the first method in the previous blog post.

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

In the above example, `then` has two callbacks. The first one is a success handler, and the second is an error handler. Using both the handlers is completely fine and works for most cases. It has certain drawbacks:

1. If the success handler ends in an error, you will not catch/handle it!
2. If you are using a chain of promises like the one in the chaining example, you will be writing an error handler for each `then` block.

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

`catch` operator does not always have to be at the very end. It could be in the middle of the chain and catch the chain's errors so far.

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

> If you are using a chain of promises like the one in the chaining example, you will be writing an error handler for each `then` block.

There will be times when you **DO** want different error handlers for all `then` blocks in your chain (maybe for easier debugging or logging). At that point, the error handler in individual `then` blocks becomes an advantage.

### Operators

There are two key operators that promises have, which are suited for specific conditions: `Promise.all` and `Promise.race`.

#### Promise.all

Promise chaining comes in handy when you want to do one async operation after another (sequentially). Quite often, you would have to do multiple async operations concurrently without waiting for one to complete. Also, your action (callback) depends on all the async operations completing.

`Promise.all` allows us to run multiple async operations simultaneously (saving us time) but still wait for all of them to complete before executing the callback.

```javascript
const waitForMe = function (name) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            return resolve(name);
        }, 2000);
    });
}

const firstPromise = waitForMe("Parwinder");
const secondPromise = waitForMe("Lauren");
const thirdPromise = waitForMe("Robert");
const fourthPromise = waitForMe("Eliu");

Promise.all([firstPromise, secondPromise, thirdPromise, fourthPromise])
    .then((data) => {
        console.log(data); // [ 'Parwinder', 'Lauren', 'Robert', 'Eliu' ]
    });
```

The example executes all promises together, and once all of them return the `name`, outputs an array of results. This execution will take 2 seconds to output four names vs. the chaining example will take 8 seconds to output all four names.

The order of output in the array is strictly the same as the order of input promises to `Promise.all`.

🚨 Even if there is a **single** failure in `Promise.all`, the result will be that rejection or failure.

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

const firstPromise = waitForMe("Parwinder");
const secondPromise = waitForMe("Lauren");
const thirdPromise = waitForMe("Robert");
const fourthPromise = waitForMe("Eliu");

Promise.all([firstPromise, secondPromise, thirdPromise, fourthPromise])
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error); // Robert is always on time
    })
```

It will ignore all the other successfully resolved promises. If there is more than one rejection, it will output the rejection from a promise that comes first in the input array of promises.

```javascript
const waitForMe = function (name) {
    return new Promise((resolve, reject) => {
        if (name === "Robert") {
            return reject("Robert is always on time");
        } else if (name === "Lauren") {
            return reject("Lauren is always on time");
        } else {
            setTimeout(() => {
                return resolve(name);
            }, 2000);
        }
    });
}

const firstPromise = waitForMe("Parwinder");
const secondPromise = waitForMe("Lauren");
const thirdPromise = waitForMe("Robert");
const fourthPromise = waitForMe("Eliu");

Promise.all([firstPromise, secondPromise, thirdPromise, fourthPromise])
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error); // Lauren is always on time
    })
```

#### Promise.race

`Promise.race` handles a unique case. When you would like to run multiple async operations at the same time, but not wait for all to complete. Instead, you want to execute callback as soon as the first one completes (hence the keyword "race").

```javascript
const waitForMe = function (name, time) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            return resolve(name);
        }, time);
    });
}

const firstPromise = waitForMe("Parwinder", 4000);
const secondPromise = waitForMe("Lauren", 3000);
const thirdPromise = waitForMe("Robert", 7000);
const fourthPromise = waitForMe("Eliu", 5000);

Promise.race([firstPromise, secondPromise, thirdPromise, fourthPromise])
    .then((data) => {
        console.log(data);
    })
    .catch((error) => {
        console.log(error); // Lauren
    })
```

I have made the `setTimeout` time an argument as well. With each name, I am passing the time. "Lauren" has the least time of 3 seconds (3000 ms) so she would always win the race, and the console outputs her name.
