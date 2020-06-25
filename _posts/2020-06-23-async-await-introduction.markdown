---
layout: post
title: "Async/Await: Introduction"
date: 2020-06-23 12:00:00 -0500
---

Callbacks and promises are great when it comes to performing async operations. Promises improved over callbacks and provide a flat code syntax, especially when it comes to chaining promises. The operators on promises like `allSettled`, `any`, `then` and `catch` make it easier to write complex async operations.

Async/Await was introduced in ES7 to promote a cleaner syntax to promises. Under the hood, async/await are promises; they provide a nice abstraction layer under those keywords.

### Async

`async` keyword could be used in front of any function (declaration, expressions, callbacks or literally anywhere). All it means is that the function will always return a promise. Any return values other than a promise will be wrapped in a resolved promise.

```javascript
async function foo() {
    return "Parwinder" // returning a string but `async` will ensure it is wrapped in a promise
}

foo().then((data) => { // we can safely use then because async function foo returns a promise
    console.log(data); // Parwinder
})
```

We could return a promise in function `foo`, and it will still work. It will be unnecessary, though.

```javascript
async function foo() {
    return Promise.resolve("Parwinder")
}

foo().then((data) => {
    console.log(data); // Parwinder
})
```

### Await

`await` keyword makes JavaScript wait until the promise **settles** and returns its result. It can only be used inside an `async` function.

```javascript
async function foo() {
    const myPromise = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("Parwinder"); // resolves with "Parwinder" after 2 seconds
        }, 2000);
    });

    // will not move to the next line until myPromise resolves/rejects
    const name = await myPromise;
    // the execution pauses (or awaits) for the promise

    console.log(name); // Parwinder
}

foo();
```

As you can see in the above example `await` provides a cleaner syntax compared to `Promise.then`.
