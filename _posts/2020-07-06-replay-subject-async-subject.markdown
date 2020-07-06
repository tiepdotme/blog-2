---
layout: post
title: "Replay & Async Subject (Observables)"
date: 2020-07-06 12:00:00 -0500
---

Now that we know what Subject and Behavior Subject is, it is time to dive into Replay and Async Subject. Both of them are variants of Subject but with slight differences. I will go over examples of each and explain the difference.

### ReplaySubject

ReplaySubject can maintain old values emitted. The behavior is useful when you want these old values replayed to a new subscriber. Saving old values is unlike BehaviorSubject. BehaviorSubject only maintains the latest of the emitted values.

You can save a set number of values, say X and the ReplaySubject will immediately emit all of the X values to a new subscriber.

```javascript
import { ReplaySubject } from 'rxjs';

const subject = new ReplaySubject(3);
// 3 is the buffer or values to hold

subject.next(1);
subject.next(2);

subject.subscribe(
  data => console.log(`Observer A: ${data}`)
  // A will have 1 and 2 (as Subject can hold up to 3 values)
);

subject.next(3); // A will have 3
subject.next(4); // A will have 4

subject.subscribe(
  data => console.log(`Observer B: ${data}`)
  // B will have last 3 values => 2, 3, 4
);

subject.next(5); // A and B both get the value 5
```

The output based on what each Observer gets is:

```console
Observer A: 1
Observer A: 2
Observer A: 3
Observer A: 4
Observer B: 2
Observer B: 3
Observer B: 4
Observer A: 5
Observer B: 5
```

### AsyncSubject

AsyncSubject is a type of Subject that only emits its last value on completion. It emits the latest value to all the Observers on `complete()`.

AsyncSubject is useful for fetching and caching one response resources, like an HTTP call to the backend. Components that subscribe after the fetch will then pick up value already fetched.

```javascript
import { AsyncSubject } from 'rxjs';

const subject = new AsyncSubject();

subject.subscribe(
    data => console.log(`Observer A: ${data}`)
);

subject.next(1); // nothing gets logged

subject.subscribe(
    data => console.log(`Observer B: ${data}`)
);

subject.next(2); // nothing gets logged
subject.next(3);

subject.complete(); // Observer A and B log the last value, 3
```

Based on our findings above, the console output will be:

```console
Observer A: 3
Observer B: 3
```

### Summary (Differences)

- `Subject` does not return the current value on subscription. It triggers only on `.next(value)` and returns the value, just like an Observable.
- `BehaviorSubject` will return the initial value or the current value of a subscription. It only maintains **one current/latest value**.
- `ReplaySubject` allows the `Subject` to holding more than one value.
- `AsyncSubject` emits **only** its last value on `complete()`.
