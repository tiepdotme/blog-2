---
layout: post
title: "Observable Operators: Merge & Concat"
image: "/assets/social/merge-concat.png"
date: 2020-07-08 12:00:00 -0500
---

### Merge

Merge as the name suggests merges all given input Observables into output Observables without doing any operations on the input. The output Observable will `complete` when all the input Observables `complete`. If any input Observables have an error, that error is sent to the output Observable immediately.

```javascript
import Rx from 'rxjs';

const interval1 = Rx.Observable.interval(1000).map(i => `first: ${i}`);
const interval2 = Rx.Observable.interval(500).map(i => `second: ${i}`);

const combinedInterval = Rx.Observable.merge(interval1, interval2).take(10);

combinedInterval.subscribe(
    data => console.log(`${data}`)
);
```

We have combined two `interval` Observables into a `combinedInterval` Observable. The first interval will output every second and the second one will do it every half second. The output will be:

```console
second: 0
first: 0
second: 1
second: 2
first: 1
second: 3
second: 4
first: 2
second: 5
second: 6
```

`merge` also allows you to specify the concurrency of merged Observable. For example, if I combine 3 `interval` based Observables but I only want two of them to run at a time, I can pass a schedule(like) parameter.

```javascript
import Rx from 'rxjs';

const interval1 = Rx.Observable.interval(1000).map(i => `first: ${i}`).take(10);
const interval2 = Rx.Observable.interval(500).map(i => `second: ${i}`).take(5);
const interval3 = Rx.Observable.interval(500).map(i => `third: ${i}`).take(10);

const combinedInterval = Rx.Observable.merge(interval1, interval2, interval3, 2); // 2 at a time

combinedInterval.subscribe(
    data => console.log(`${data}`)
);
```

First and second Observable run concurrently until the five values from the second Observable has been emitted. Then the third Observable joins the first one in emitting values. The output will look like:

```console
second: 0
first: 0
second: 1
second: 2
first: 1
second: 3
second: 4
first: 2
third: 0
third: 1
first: 3
third: 2
third: 3
first: 4
third: 4
third: 5
first: 5
third: 6
third: 7
first: 6
third: 8
third: 9
first: 7
first: 8
first: 9
```

### Concat

Concat is slightly different from the merge. Both combine the Observables, but `concat` waits for one Observable to complete before it starts the next one. All the Observables concatenated run in sequence. Whereas in case of a merge, all of them run but provide a single stream of output.

```javascript
import Rx from 'rxjs';

const interval1 = Rx.Observable.interval(1000).map(i => `first: ${i}`).take(5);
const interval2 = Rx.Observable.interval(500).map(i => `second: ${i}`).take(5);

const combinedInterval = Rx.Observable.concat(interval1, interval2);

combinedInterval.subscribe(
    data => console.log(`${data}`)
);
```

So `concat` will combine the output but maintain the output order. The console log will be:

```console
first: 0
first: 1
first: 2
first: 3
first: 4
second: 0
second: 1
second: 2
second: 3
second: 4
```
