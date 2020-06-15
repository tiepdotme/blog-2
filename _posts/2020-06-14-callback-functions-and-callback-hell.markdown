---
layout: post
title:  "Callback Functions & What is Callback Hell?"
image: "/assets/social/callbacks.png"
description: "Callback functions are great for async programming in JavaScript, but they do come with some drawbacks."
date:   2020-06-14 12:00:00 -0500
---

## Callback functions

We did touch upon the topic of callback functions when we discussed [event handlers](https://bhagat.me/blog/2020/06/04/events-and-event-handlers.html). This blog post will look deeper into callback functions, how they promote async programming in JavaScript, the disadvantages, and what is callback hell.

A callback function is a function passed to another function as an argument. The callback function can then be invoked inside the called function to do some action.

```javascript
function greeting(name) {
    console.log(`Hello ${name}`);
}

function getUserName(callback) {
    const name = prompt("Enter your name");
    callback(name);
}

getUserName(greeting);
```

What is happening in the example?

1. `getUserName` gets called with an argument. The argument is `greeting` function.
2. `getUserName` prompts the user to enter their name and saves it in the variable `name.`
3. `getUserName` calls the callback function with the saved `name.` It knows about the callback function because we passed it as an argument.
4. We can call the argument anything we like. It does not have to be called callback.
5. Callback(`greeting`) gets executed with `name` and prints "Hello name" to the console.

Above is a simple example of a callback function and specifically, synchronous callback. Everything gets executed line by line, one by one.

### Sync vs. Async

🚨 JavaScript is a single-threaded language. It means only one thread executes the code.

Other languages can spin up multiple threads and execute multiple processes simultaneously, but JavaScript is incapable of doing so. It could be a significant drawback when performing time-intensive operations like disk I/O or network requests.

Since only one thing can execute at a time, the user will have to wait for these time-intensive tasks to complete before they take any further action.

The JavaScript event loop, callback stack, callback queue, and web APIs help make JavaScript asynchronous.

1. JavaScript maintains a stack to pick up things to execute.
2. Actions that can take a more extended amount of time are delegated to web APIs.
3. Once time-intensive actions are ready, it is put into the callback queue.
4. As soon as the JavaScript engine does not have anything to execute in the stack, it will fetch items from the queue, move it to the stack and execute it.

### How callbacks promote async programming

There are plenty of time-intensive operations like disk I/O, network requests, and data processing. These should be performed in async fashion (or non-blocking). We can go over a more straightforward example to demonstrate this.

```javascript
console.log("Hello");
console.log("Hey");
console.log("Namaste");
```

When we run the code, the console displays, "Hello, Hey, Namaste." It is done correctly in that order. Now let us introduce `setTimeout` for the word "Hey." We want JavaScript engine to wait 2 seconds before displaying the word "Hey."

```javascript
console.log("Hello");

setTimeout(() => {
    console.log("Hey");
}, 2000);

console.log("Namaste");
```

To our surprise, it prints out, "Hello, Namaste, Hey." The expectation was to print "Hello," wait for two seconds, print "Hey," and then print "Namaste."

1. The callback function passed to `setTimeout` gets executed after 2 seconds.
2. So instead of waiting for 2 seconds, the JavaScript event loop delegates it to web APIs.
3. It sits there for 2 seconds and then moved to the callback queue.
4. In the meantime, the last console log gets executed.
5. Once there is nothing else to execute in the stack, the `setTimeout` log is pulled from the queue and moved to the stack and then executed.

P.S. Quick side note. Even if the time in `setTimeout` is 0 ms, we would see "Hello, Namaste, Hey" and not "Hello, Hey, Namaste." It is surprising because 0 ms should mean, the code gets executed in now. That is not the case. It still goes through the same cycle as mentioned above, and while it is going through the queue, the last log statement gets executed. Try executing the code below:

```javascript
console.log("Hello");

setTimeout(() => {
    console.log("Hey");
}, 0);

console.log("Namaste");
```

### Disadvantages and callback hell

Callbacks get much hate because we have better ways of handling async operations. There is absolutely no need for such hatred. Callbacks work great when we have 1-2 async operations. There is nothing wrong with them, and we can use them with confidence.

Callbacks have real disadvantages the moment we need to deal with more than two async operations chained. Let us dive into an example.

Let us say that we want to log each of the greetings but with a 2-second gap between each one. It should print, "Hello, Hey, Namaste, Hi, Bonjour."

```javascript
setTimeout(() => {
    console.log("Hello");
    setTimeout(() => {
        console.log("Hey");
        setTimeout(() => {
            console.log("Namaste");
            setTimeout(() => {
                console.log("Hi");
                setTimeout(() => {
                    console.log("Bonjour");
                }, 2000);
            }, 2000);
        }, 2000);
    }, 2000);
}, 2000);
```

The cascading code above is called callback hell. **It is hard to debug and add error handling to**. It also reduces code readability. There are other names used for this callback hell like a pyramid of doom, or the Christmas tree from hell (cause it looks like a Christmas tree from the side).

I will leave with an image that will promptly remind everyone of callback hell in the future. In the next few blog posts, we will discuss other async programming methodologies (promises, async/await, and observables).

![Callback Hell Hadouken](/blog/assets/callback-hadouken.jpeg "Callback Hell")
