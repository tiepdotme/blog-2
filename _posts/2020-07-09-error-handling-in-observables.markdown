---
layout: post
title: "Error Handling in Observables"
date: 2020-07-09 12:00:00 -0500
---

The two key concepts you would want to know to handle errors in Observables are: `catch` and `retry`. As the name suggests `catch` allows us to **catch** the errors and `retry` will enable us to **retry** the action in case of an error.

### Catch

Catch does not prevent the error from happening. It merely allows us to catch the error and do something with that error. Generally, we can wrap that error in an Observable so that the Observable chain can continue. We could also display that error to the end-user in the catch block while we continue the Observable chain.

Let us go through some examples to understand this better. I will take the example of concatenating two intervals from my previous blog post. This time I will add a third Observable that produces an error.

```javascript
import Rx from 'rxjs';

const interval1 = Rx.Observable.interval(1000).map(i => `first: ${i}`).take(5);
const errorObservable = Rx.Observable.throw(new Error("An error occurred, cannot proceed"));
const interval2 = Rx.Observable.interval(500).map(i => `second: ${i}`).take(5);

const combinedInterval = Rx.Observable.concat(interval1, errorObservable, interval2);

combinedInterval.subscribe(
    data => console.log(`${data}`)
);
```

The output will be:

```console
first: 0
first: 1
first: 2
first: 3
first: 4
Error: An error occurred, cannot proceed
```

The output is as expected. We got an error after the first Observable, so the second one never ran. The problem is, what if we still wanted to continue the `concat` chain despite the error? In this situation is where `catch` comes in. We will modify the example to use `catch` and display a message.

```javascript
import Rx from 'rxjs';

const interval1 = Rx.Observable.interval(1000).map(i => `first: ${i}`).take(5);
const errorObservable = Rx.Observable.throw(new Error("An error occurred, cannot proceed")).catch(e => Rx.Observable.of(e.message));
const interval2 = Rx.Observable.interval(500).map(i => `second: ${i}`).take(5);

const combinedInterval = Rx.Observable.concat(interval1, errorObservable, interval2);

combinedInterval.subscribe(
    data => console.log(`${data}`)
);
```

Since now we have a `catch` statement for any error in the Observable, it returns a regular Observable with an error message. The output will be:

```console
first: 0
first: 1
first: 2
first: 3
first: 4
An error occurred, cannot proceed
second: 0
second: 1
second: 2
second: 3
second: 4
```

We see the execution of all Observables despite the error in one of them.

P.S. The `concat` example might not be the best example to demonstrate `catch`. Don't lose hope! I use `catch` and `retry` both to explain `retry` in the next section.

### Retry

`retry` method retries the Observable that generated the error. `retry` is helpful in case you are making an API call and you would like to keep retrying until you get success. Key things to remember about `retry`.

1. By default, it will retry infinite times.
2. It does take a numerical argument if you would like to limit the number of retries.
3. Do not use retry if you are converting a promise into an Observable (explained below).
4. `retry` truly unsubscribes from an error generating Observable and subscribes again.
5. A retry needs to execute the generator function of the Observable **again**. So, retry is only useful in case of cold Observables.
6. Hot Observable retry will not invoke the generator again, so it is useless.

When we resubscribe to a `fromPromise`, it still caches the resolve/reject status of the promise. It does not invoke the complete action again. This is why `retry` does not work with Observables made from promises.

To showcase an example of `retry`, I will create a function called `dummyApi`. The function will mimic an API call to the backend and return an error Observable. We will try and `catch` the error as well as `retry` the call.

#### Without catch or retry
```javascript
import Rx from 'rxjs';

const dummyApi = () => {
    return new Rx.Observable(observer => {
        setTimeout(() => {
            observer.error(new Error("API call failed. Sorry!")); // API call responds with an error
        }, 1000); // API call takes 1 second to respond
    });
}

dummyApi()
    .do(() => console.log("Executing next Observable, chain continues"))
    .subscribe(
        data => console.log(data),
        error => console.log(error.message) // We handle error here by displaying the message
    )
```

The output will be:

```console
API call failed. Sorry!
```

We logged the error message, but the problem is `do` operator in the chain never got executed. Now we use the `catch` operator (and this is a better example as I promise 😉).

#### Without retry (with catch)

```javascript
import Rx from 'rxjs';

const dummyApi = () => {
  return new Rx.Observable(observer => {
    setTimeout(() => {
      observer.error(new Error("API call failed. Sorry!"))
    }, 1000);
  });
}

dummyApi()
  .catch(err => Rx.Observable.of(err.message)) // Wrap the error in a regular Observable so chain continues
  .do(() => console.log("Executing next Observable, chain continues")) // `do` operator logs the message
  .subscribe(
    data => console.log(data) // The error wrapped in a regular observable could not be logged
  )
```

The output will be:

```console
Executing next Observable, chain continues
API call failed. Sorry!
```

Much better but we are still not retrying!

#### With retry and catch!

```javascript
import Rx from 'rxjs';

const dummyApi = () => {
    return new Rx.Observable(observer => {
        console.log("Calling API"); // Added the log to display retry
        setTimeout(() => {
            observer.error(new Error("API call failed. Sorry!"))
        }, 1);
    });
}

dummyApi()
    .retry(3) // Retry 3 times
    .catch(err => Rx.Observable.of(err.message))
    .do(() => console.log("Executing next Observable, chain continues"))
    .subscribe(
        data => console.log(data)
    )
```

I added a console log statement to `dummyApi` so we can see retry attempts. The output will be:

```console
Calling API
Calling API
Calling API
Calling API
Executing next Observable, chain continues
API call failed. Sorry!
```

The API gets called, it fails and then it is retried three more times. That is why we see four logs with "Calling API" (original call plus three retries).

The above code handles retries, logs the error message if any, and continues the chain of Observable operators. Voila!

Happy coding 👋🏼
