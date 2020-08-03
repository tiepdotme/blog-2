---
layout: post
title: "Basics of Promises"
image: "/assets/social/promises.png"
date: 2020-06-20 12:00:00 -0500
---

### Introduction

[Callbacks](https://bhagat.me/blog/2020/06/14/callback-functions-and-callback-hell.html) are great to do operations that take the time or could be blocking in nature. We saw how they have certain drawbacks, especially callback hell.

To overcome the drawbacks of callbacks, we introduced promises. The critical difference between the two is when using callbacks, we would pass a callback into a function that gets called upon completion, and we get the result. In the case of promises, we do a callback on the returned promise!

#### Advantages

1. Promises and callbacks achieve the same thing when it comes to asynchronous operations. Promises add a layer of abstraction that allows cleaner, functional, and less error-prone code.
2. We are not required to know the callback that will use the value of the async operation
3. Promises are chain-able while keeping flat structure to the code and not causing callback hell.
4. They do come with in-built error handling.

### Creation

We create promises using the `Promise` constructor.

```javascript
const myPromise = new Promise();
```

A promise is like an IOU that says I will have a value for you in the future. Once the promise completes (resolves with success or rejects with an error), we can take action (such as employee data fetched from a backend).

### Resolve

A promise could take whatever time it needs to complete an asynchronous task. While the asynchronous task is being performed, the promise is in **pending** state. Once it completes the operation, it **resolves** (generally with the data from the async task).

```javascript
const myPromise = new Promise((resolve) => {
    setTimeout(() => {
        resolve("finished async operation");
    }, 2000);
});

myPromise.then((response) => {
    console.log(response); // finished async operation
});
```

What is happening here?

1. Promise takes a callback function
2. This callback performs the async operation
3. If the task is successful, the promise is **resolved**
4. We are using setTimeout to simulate an async task that takes 2 second
5. When 2 seconds complete or the async task finishes, we resolve with the message or The data brought back by async operation

### Reject

There are times when the async task will not complete as expected. We might run into an error. In this case, we use the `reject` function to notify about the failure.

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

The callback in promise takes two methods: `resolve` and `reject`. The `then` operator on a promise is capable of handling two callbacks. The first one is for success (resolve) and the second one for error (reject).

In this example, we ran into an error at the 2-second mark. We informed whoever was using the `myPromise` promise that hey, "an error has occurred".

The post covered the basics of promises. In the next blog post, we will go over chaining, error handling, and executing multiple promises in parallel.

👋🏼
