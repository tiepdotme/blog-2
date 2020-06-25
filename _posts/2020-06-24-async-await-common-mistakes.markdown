---
layout: post
title: "Async/Await: Common Mistakes"
date: 2020-06-24 12:00:00 -0500
---

### What await cannot do

Before we get too comfortable using `await` in our code we need to realize that we **cannot**:

1. Use `await` in a function that is not marked `async`. You have to prefix the function with `async` keyword if you are going to use `await` inside it.
2. Use `await` on the top level.

We discussed the first item in [previous blog post](http://bhagat.me/blog/2020/06/23/async-await-introduction.html). For the second one here is an example:

```javascript
async function wait(message, time) {
    return new Promise((resolve) => setTimeout(resolve(message), time));
}

await wait ("hello", 2000); // SyntaxError: await is only allows inside an async function
```

We can rewrite this to make it work correctly.

```javascript
async function wait(message, time) {
    return new Promise((resolve) => setTimeout(resolve(message), time));
}

async function execute() {
    const message = await wait ("hello", 2000);
    console.log(message); // hello (after 2000 ms)
}

execute();
```

### Accidentally making code synchronous

The biggest issue with async/await is the `await` keyword and how it is easy to misuse it. We almost always want our code to run asynchronously (if we have the option) and make sure we do not block the client.

To help us understand this, let's start with a promise example, convert it into async/await and then correct a mistake that happens far too often.

```javascript
const sayGreeting = (name, time) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(`Hello ${name}`);
        }, time);
    })
}

sayGreeting("Parwinder", 1000)
    .then((data) => {
        console.log(data); // "Hello Parwinder" after 1 second
        return sayGreeting("Lauren", 2000);
    })
    .then((data) => {
        console.log(data); // "Hello Lauren" after 2 seconds
        return sayGreeting("Robert", 500);
    })
    .then((data) => {
        console.log(data); // "Hello Robert" after half a second
        return sayGreeting("Eliu", 2000);
    })
    .then((data) => {
        console.log(data); // "Hello Eliu" after 2 seconds
        return sayGreeting("George", 1500);
    })
    .then((data) => {
        console.log(data); // "Hello George" after 1.5 seconds
    })
```

The above example says a greeting to a person after a specified time. Promises made the code flat as compared to callbacks, but this is still chained code with at least one callback in each link.

An individual that has recently learned `await` might rewrite this like so:

```javascript
const sayGreeting = (name, time) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(`Hello ${name}`);
        }, time);
    })
}

const main = async () => {
    let a = await sayGreeting("Parwinder", 1000);
    console.log(a); // "Hello Parwinder" after 1 second
    a = await sayGreeting("Lauren", 2000);
    console.log(a); // "Hello Lauren" after 2 seconds
    a = await sayGreeting("Robert", 500);
    console.log(a); // "Hello Robert" after half a second
    a = await sayGreeting("Eliu", 2000);
    console.log(a); // "Hello Eliu" after 2 seconds
    a = await sayGreeting("George", 1500);
    console.log(a); // "Hello George" after 1.5 seconds
}

main();
```

No more `then` callbacks and a lot easier to read. So far, we have created a promise and converted it into async/await. The converted code looks a lot better, so where is the mistake?

More often than not, we can do async operations in parallel. Every time I write an `await` statement in the `main` I am making JavaScript wait for that promise to complete and then move forward. We could probably execute all five promises at the same time and get back the greetings.

The first promise example I provided is chained/sync, as well. So if you have read my [previous blog posts](http://bhagat.me/blog/2020/06/21/advanced-promises-chaining-error-handling.html) on promises, you would know how we run multiple promises at the same time! We use `Promise.all` and that is what we are going to do with the async/await example to make it performant.

```javascript
const sayGreeting = (name, time) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(`Hello ${name}`);
        }, time);
    })
}

const main = async () => {
    const a = sayGreeting("Parwinder", 1000);
    const b = sayGreeting("Lauren", 2000);
    const c = sayGreeting("Robert", 500);
    const d = sayGreeting("Eliu", 2000);
    const e = sayGreeting("George", 1500);
    const [greeting1, greeting2, greeting3, greeting4, greeting5] = await Promise.all([a, b, c, d, e]);
    // all promises in promise.all
    console.log(greeting1, greeting2, greeting3, greeting4, greeting5)
}

main();
```

What did we do here:

1. Instead of awaiting for each promise, we stored the promise in a variable.
2. Created a mega promise that has `all` promises passed to it.
3. We `await` this `Promise.all` instead of individual promises.
4. `Promise.all` executes all promises at the same time and when all of them finish, assigns the response to variables
5. We log the results 🙂

I hope this improves your ability to use async/await. We will learn about error handling with async/await in the next blog post.

Until then, happy coding. 👋🏼
