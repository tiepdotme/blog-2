---
layout: post
title: "Creating Observables: Part 2"
date: 2020-07-02 12:00:00 -0500
---

We went over how to create Observable in the last post. This blog post is a continuation of that. We will go over how we can unsubscribe from an Observable, how do we clean up code executed by Observable, and chaining operators when we subscribe.

### Unsubscribing from Observables

We invoke `unsubscribe()` function to release resources or cancel Observable executions. If we do not unsubscribe from the subscriptions when we are done with it, we risk memory leaks. The `unsubscribe()` method comes with the subscription.

```javascript
import { interval } from 'rxjs/observable/interval';

const observable = interval(1000);
const subscription = observable.subscribe(x => console.log(x));
setTimeout(() => {
    subscription.unsubscribe();
}, 4500);
```

What is going on here:

1. We have imported `interval` to create an Observable. `interval` creates an Observable that emits sequential numbers every provided **interval** of time.
2. `observable` is the Observable created using `interval`.
3. We subscribe to the `observable` and log the value (sequence of numbers);
4. We save this subscription to a variable `subscription`.
5. This variable is a reference to subscription, and this is how we unsubscribe. It is similar to how we reference setTimeout or setInterval when we would like to clear them.
6. After 4.5 seconds, I `unsubscribe()` from the subscription, essentially killing it and freeing up resources.
7. Since we terminate at 4.5 seconds, and we have an interval of 1 second, the Observable executes four times before ending.
8. Our output will be `0, 1, 2, 3`.

### Cleaning up Observable

An Observable could be executing code internally that would need cleanup. If we return a function from the Observable, the function gets executed on unsubscribe. We can use this return function for cleanup.

```javascript
import Rx from 'rxjs';

const observable = new Rx.Observable(observer => {
    let count = 0;
    setInterval(() => {
        console.log("Starting setInterval");
        observer.next(count++);
    }, 1000);
});

const subscription = observable.subscribe(
    data => console.log(data)
);

setTimeout(() => {
    subscription.unsubscribe();
}, 4500);
```

In the above example, we are doing the same thing as our first example. We are printing out sequential numbers. Instead of using the `interval` method to create an observable, I have used the classic `setInterval` this time. You will understand why I did this soon. I have also added a console log to `setInterval`.

Once I created the `observable`, I subscribe to it and save the subscription in the `subscription` variable. After 4.5 seconds, I unsubscribe (being the responsible developer I am).

The output will be:

```console
Starting setInterval
0
Starting setInterval
1
Starting setInterval
2
Starting setInterval
3
Starting setInterval
Starting setInterval
Starting setInterval
Starting setInterval
Starting setInterval
```

We get numbers 0 to 3 as expected. The behavior is the same as our previous example. We also get the statement "Starting setInterval" before each number because I added a console log.

**The problem is** that even when I unsubscribe from the Observable, I never cleaned up the `setInterval`. So although the Observable is dead, setInterval was never cleared using clearInterval! This statement will be printed an infinite number of times or until the process exits.

We can solve this by return a function from the Observable that gets executed automatically on unsubscribe.

```javascript
import Rx from 'rxjs';

const observable = new Rx.Observable(observer => {
    let count = 0;
    const interval = setInterval(() => {
        console.log("Starting setInterval");
        observer.next(count++);
    }, 1000);

    // this returned function executes on unsubscribe
    return () => {
        clearInterval(interval)
    }
});

const subscription = observable.subscribe(
    data => console.log(data)
);

setTimeout(() => {
    subscription.unsubscribe();
}, 4500);
```

### Chaining an Operator

So far, we have used a setTimeout to kill our subscription. We have the time set to 4500ms to get the first four values as they are spread 1 second apart. We can do better! Operators allow us to perform operations on Observables and return a new Observable.

```javascript
import { take } from 'rxjs/operators';
import { interval } from 'rxjs/observable/interval';

const observable = interval(1000);

observable
    .pipe(  // allows us to chain operators before we perform the core operation like subscribe
        take(4) // take operator take x number of values from Observable
    ).subscribe(
        data => console.log(data) // we output the first 4 values: 0, 1, 2, 3
    );
```
