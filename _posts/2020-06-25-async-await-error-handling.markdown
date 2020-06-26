---
layout: post
title: "Async/Await: Error Handling"
date: 2020-06-25 12:00:00 -0500
---

Promises allowed us to catch the error by using an error handler in `then` block or using a `catch` block. Async/await has similar strategies.

### Using catch with await

We `await` an `async` function (mostly, `await` works with anything that produces **thenable**). `Async` functions provide promises so we can still take advantage of a `catch` block.

```javascript
const myPromise = async () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject("We ran into an error");
        }, 2000);
    });
}

const main = async () => {
    const response = await myPromise().catch((err) => {
        console.log(err); // We ran into an error
    })
}

main();
```

`myPromise` gets rejected after 2 seconds with a message. When we are awaiting for this promise, we can chain a `catch` block to handle the error.

### Using catch when calling the function

We can also add the catch block when we are calling the async function.

```javascript
const myPromise = async () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject("We ran into an error");
        }, 2000);
    });
}

const main = async () => {
    const response = await myPromise();
}

main().catch((err) => {
    console.log(err); // We ran into an error
});
```

Since `main` is an `async` function, it will return a promise so we can make use of a `catch` block! Using `catch` is smart, but it has a disadvantage. It will catch any error happening in the `main` function and not only the error while awaiting `myPromise`.

So if you had more code in the `main` function that could result in an error, this `catch` block would get that too.

### Using a higher-order function

We can add such `catch` blocks when calling the function but imagine if you have a large number of async function that you call in your application. Adding catch block to each one of them will become tiring. You do need to handle errors, though.

This is where a higher-order function will come into play. A higher-order function takes a function as an input and returns a function. These are used to transform the input function (in simple terms).

```javascript
const myPromise = async () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject("We ran into an error");
        }, 2000);
    });
}

const main = async () => {
    const response = await myPromise();
}

const handleError = (err) => {
    console.log(err); // We ran into an error
}

const addingErrorHandler = (fn, errorHandler) => {
    return function() {
        fn().catch(errorHandler);
    }
}

const mainFunctionWithErrorHandler = addingErrorHandler(main, handleError);

mainFunctionWithErrorHandler();
```

We have added three new things:

1. `handleError` as a standard function to handle errors.
2. `addingErrorHandler` as a higher-order function. It takes a function and adds error handling to it.
3. `mainFunctionWithErrorHandler` converts the `main` function to a function that can handle error using our higher-order function.

Adding all the functions might seem like a lot of code at the moment because we are doing this for one function `main`. We will be able to reuse this error handler and higher-order function for x number of async functions in a large application.

### Using a try/catch block

JavaScript provides us with try/catch block where you try a block of code and if an error occurs, catch it in the `catch` block. We can use this with async/await as well.

```javascript
const myPromise = async () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject("We ran into an error");
        }, 2000);
    });
}

const main = async () => {
    try {
        await myPromise(); // try this code
    } catch (err) { // if it throws an error execute the catch block
        console.log(err); // We ran into an error
    }
}

main();
```

Try/catch is the easiest to technique to understand and implement. It is excellent in a not so complex application, but I do prefer higher-order functions as the application becomes bigger.
