---
layout: post
title: "Hot vs Cold Observables"
date: 2020-07-07 12:00:00 -0500
---

Observables do not have types like hot and cold Observables. We do not explicitly state if an Observable is hot or cold. Hot and cold Observables is how the Observable works and emits values to its subscribers (or in the absence of subscribers).

Hot Observables push events or values even when no one is subscribed to them. For example, if you have created an Observable based on mouse clicks or an Observable for keyup event in a search bar, it will keep on producing values regardless of if someone is subscribed for those values. You have to keep in mind that your subscriber or observer will get values from a hot Observable starting from the moment they subscribed. Any values emitted before subscription **will be lost**.

Cold Observables only emit values on subscription. An HTTP call to an endpoint is an example of a cold Observable. It will call the endpoint when you subscribe and get the value from the backend. No values are missed in case of a cold Observable. They also re-do everything every time you subscribe.

**Real life example**

Let's say you want to watch a movie that is in theaters, and it is also being streamed on Netflix.

You can watch the movie on Netflix anytime. Whenever you do decide to watch the film, you will begin watching it from the beginning, and you can watch the complete film until the end. It is similar to a cold Observable.

Compare this to the showtime of the same movie at 4 PM. If you do not show up at 4 PM you will miss part of the film. You are the subscriber, and you are bound to miss part of the movie depending on when you show up. It is a hot Observable. It does not care when someone subscribes.

**Code example**

Let's start with a cold Observable example that you have seen in the past blog posts.

```javascript
import Rx from 'rxjs/Rx';
import { interval } from 'rxjs/observable/interval';
import { take } from 'rxjs/operators';

// create an Observable
// emits sequential values from 0-4 (take first 5)
const observable = Rx.Observable.interval(1000).take(5);

setTimeout(() => {
    observable.subscribe(
        // first subscriber starts in 3 seconds
        value => console.log(`observer A: ${value}`)
    )
}, 3000);

setTimeout(() => {
    observable.subscribe(
        // second subscriber starts in 5 seconds
        value => console.log(`observer B: ${value}`)
    )
}, 5000);
```

Two subscribes A and B start at 3 and 5 seconds respectively. Until then, `observable` does not produce any value. Both Observable A and B receive all five values from number 0 to 4. They are not missing any values.

Output:

```console
observer A: 0
observer A: 1
observer B: 0
observer A: 2
observer B: 1
observer A: 3
observer B: 2
observer A: 4
observer B: 3
observer B: 4
```

To understand hot Observables, rather than doing a brand new example, we will convert the existing `interval` Observable into a hot Observable.

To do so, we will use the `publish` method. The `publish` method returns a connectable Observable. The connectable Observable does not subscribe to the underlying Observable until we use the `connect` method.

```javascript
import Rx from 'rxjs/Rx';
import { interval } from 'rxjs/observable/interval';
import { take } from 'rxjs/operators';

// adding publish returns a connectable Observable
// making this a hot Observable
const observable = Rx.Observable.interval(1000).take(5).publish();

setTimeout(() => {
  observable.subscribe(
    value => console.log(`observer A: ${value}`)
  )
}, 3000);

setTimeout(() => {
  observable.subscribe(
    value => console.log(`observer B: ${value}`)
  )
}, 5000);
```

The above code **will not produce anything** until we `connect` the `observable`.

```javascript
import Rx from 'rxjs/Rx';
import { interval } from 'rxjs/observable/interval';
import { take } from 'rxjs/operators';

const observable = Rx.Observable.interval(1000).take(5).publish();

// as soon as we connect it starts producing values
observable.connect();

setTimeout(() => {
  observable.subscribe(
    // observer A starts at 2100 ms so it will lose the first two values
    // it will get 2, 3, 4
    value => console.log(`observer A: ${value}`)
  )
}, 2100);

setTimeout(() => {
  observable.subscribe(
    // observer B starts at 4100 ms so it will lose the first four values
    // it will only get 4 and miss 0 to 3
    value => console.log(`observer B: ${value}`)
  )
}, 4100);
```

You can see that `observable` starting emitting values on `connect`. Observer A started at 2.1 seconds, so it lost the first two values. In the same way, Observer B started at 4.1 seconds, missing the first four values and only getting the last value of 4.

```console
observer A: 2
observer A: 3
observer A: 4
observer B: 4
```
