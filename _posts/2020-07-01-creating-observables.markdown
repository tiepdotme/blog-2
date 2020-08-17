---
layout: post
title: "Creating Observables"
image: "/assets/social/create-obs.png"
date: 2020-07-01 12:00:00 -0500
---

### Creation

We should be caught up on Observables, Operators & Subscriptions by now. If not, please read the last two blog post [here](https://bhagat.me/blog/2020/06/29/what-are-observables.html) and [here](https://bhagat.me/blog/2020/06/30/operators-subscriptions-observables.html).

Now we will go over creating observable and look at the technical definition and working of one.

1. Observables are generator functions that take in an observer.
2. This observer invokes three events: next, error and complete.
3. The next event defines what to generate next.
4. Error event handles any error in the process.
5. The complete event does not provide any data, but it is an event that tells the subscriber that the Observable will

```javascript
import Rx from "rxjs/Rx";

const myObservable = new Rx.Observable(observer => {
  console.log("Starting Subscription"); // Logs as the first statement when Observable is subscribed
  setTimeout(() => { // Mimic async operation that takes 1 second using setTimeout
    observer.next("First Item"); // We get "First Item" as the first value from
    setTimeout(() => {
      observer.next("Second Item"); // A second later, we get the second value "Second Item"
      observer.complete(); // Eventually, Observable completes operation
    }, 1000)
  }, 1000);
});
```

Above is an Observable that provides two values, one second apart and then marks as complete. Keep in mind:

1. The observable will not generate any value until it is subscribed.
2. Every time we subscribe to this Observable, it will rerun the generator function and provide the values to the new subscriber.

### Subscription

We can subscribe (link the Observable and the subscriber) using the `subscribe` operator. It takes 0 to 3 function. The first function maps to next, second to error and the last one maps to the complete event.

```javascript
import Rx from "rxjs/Rx";

const myObservable = new Rx.Observable(observer => {
  console.log("Starting Subscription");
  setTimeout(() => {
    observer.next("First Item");
    setTimeout(() => {
      observer.next("Second Item");
      observer.complete();
    }, 1000)
  }, 1000);
});

myObservable.subscribe(
  data => console.log(data), // next
  error => console.log(error), // error
  () => console.log("Completed!") // complete
);
```

The above code will output:

```console
Starting Subscription
First Item
Second Item
Completed!
```

We can have multiple subscribers to an Observable!

```javascript
import Rx from "rxjs/Rx";

const myObservable = new Rx.Observable(observer => {
  console.log("Starting Subscription");
  setTimeout(() => {
    observer.next("First Item");
    setTimeout(() => {
      observer.next("Second Item");
      observer.complete();
    }, 1000)
  }, 1000);
});

myObservable.subscribe(
  data => console.log(`Subscriber 1: ${data}`),
  error => console.log(`Subscriber 1: ${error}`),
  () => console.log("Subscriber 1 Completed!")
);

myObservable.subscribe(
  data => console.log(`Subscriber 2: ${data}`),
  error => console.log(`Subscriber 2: ${error}`),
  () => console.log("Subscriber 2 Completed!")
);
```

The output will be:

```console
Starting Subscription
Starting Subscription
Subscriber 1: First Item
Subscriber 2: First Item
Subscriber 1: Second Item
Subscriber 1: Completed!
Subscriber 2: Second Item
Subscriber 2: Completed!
```

### Error

The Observable can produce an error, and we should be able to handle it. Whenever an error event happens, the second handler (error) of our subscriber will do what we need.

```javascript
import Rx from "rxjs/Rx";

const errorObservable = new Rx.Observable(observer => {
  observer.error(new Error("We have encountered an error"));
});

errorObservable.subscribe(
  data => console.log(data),
  error => console.log(error.message) // "We have encountered an error"
);
```

`observer.error` could return anything—even a string. We have user the `Error` constructor and passed a custom message. We can access the message using `error.message`. It is handy if we would like to see the stack trace of why the error happened. We can use `error.stack`.
