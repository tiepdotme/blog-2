---
layout: post
title: "What are, Observables?"
date: 2020-06-29 12:00:00 -0500
---

### Introduction

Observables are things that you can observe. It is something that produces values over time. Observables can depict different behaviors depending on what you are using it for. It can:

1. Produce values non-stop (forever).
2. It can produce value and die.
3. It can produce an error and die.
4. It can provide values for a small duration, pause and again start sending data.

You get the idea. Observable allows you to handle different asynchronous events like a single operation (HTTP requests) or multiple repeatable actions (like cursor movements or keypresses). It is relatively flexible in operation (and it is meant to be so).

### Why Observables?

The description of observables I provided in the introduction is a relatively high level/vague. As we go further in this post things will get clearer.

The most significant need for Observables arises from async operations. In this blog, I have discussed callbacks, promises, and async/await. Promises, callbacks and async/await handle asynchronous data well, but when it comes to asynchronous **stream** of data, observables is king.

Observables come into the picture with RxJS library, and they introduce reactive programming. Reactive programming is a method of building applications which will react to changes that happen instead of writing applications where we write code to handles those changes (or imperative programming).

To understand how observables work, we need to understand the two communication strategies between producer and consumer of data.

### Pull vs Push Model

Pull and push models defines how a producer of data works with a consumer of data.

**Pull**: In the case of a pull model, the consumer is deciding when data is consumed or asked for. When you create a function that returns a value, that function is a producer. However, that function does not produce anything until the function is **called** (or asked for data).

The piece of code that is calling the function is the consumer. This call happens on-demand (or when needed). The consumer decides the communication strategy.

**Push**: The producer dominates the push model. Anyone consuming data is not aware when the data will arrive. They know what to do when data arrives, but they do not decide the timing.

Promises are a classic example of a push model. A promise can **produce** data or error when the task completes. The callback function passed to the promise is never aware of **when** the promise will complete. It does, however, handles success or error state.

### Examples of Observables

1. Asynchronous HTTP Request
   ```javascript
    const getEmployees = () => { // A method to get employee data
        let content; // Variable to store the retrieved data
        const url = "https://www.example.com/getEmployeeData"; // Mock url where we get data from
        return this.http.get(url).subscribe(results => contents = results);
        // this.http is the Angular (for this example) HTTP library you injected into your class to make async requests
        // It calls the URL and returns an observable
        // We subscribe to that observable to get the data
        // No request is made until there is a subscription in case of **cold** observables
    }
   ```
2. Mouse events: These could be clicks or hover or any other event from a mouse. Since an end-user is browsing your site, you will have multiple events over time.
3. Key presses: Similar to mouse events. One of the common examples is a search bar where you have to continually make async requests when a user is typing a search query to suggest results.

