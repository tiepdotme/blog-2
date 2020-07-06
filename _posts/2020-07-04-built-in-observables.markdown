---
layout: post
title: "Built-in Observables"
date: 2020-07-04 12:00:00 -0500
---

There are multiple ways of creating Observables in RxJS. We went through a couple of examples in the last few blog posts. We will go over a few more in this one. Some of them are essential, some based on time and some are what I consider meh ¯\\_(ツ)_/¯.

### Essentials!

#### of

`of` creates an Observable from arguments. It does not do any flattening of arguments. If you pass it an Array-like argument, it will not iterate over the argument to generate Observable sequence. Instead, it will emit the whole argument.

```javascript
import { of } from 'rxjs';

of(10, 20, 30)
    .subscribe(
        value => console.log(value) // 10 20 30 (3 log ouputs)
    );

of([10, 20, 30])
    .subscribe(
        value => console.log(value) // [10, 20, 30] (1 log output, no looping of array)
    );
```

#### from

`from` creates an Observable from Array, Array-like or iterable object. `from` iterates over the argument to provide a sequence of emitted values unlike `of`.

```javascript
import { from } from 'rxjs/observable/from';

const array = [10, 20, 30, 40];
const numbers = from(array);

numbers.subscribe(
    value => console.log(value)
);
```

As I said, it can work with more than Arrays. We will take an example where we pass it a generator function.

```javascript
import { from } from 'rxjs/observable/from';
import { take } from 'rxjs/operators';

// starts with 2
// doubles the number between yields
function* generator() {
   let i = 2;
   while (i <= 32) {
     yield i;
     i = i * 2;
   }
}

const iterator = generator();
const numbers = from(iterator);

numbers.subscribe(x => console.log(x)); // 2 4 8 16 32
```

#### throw

`throw` creates an Observable that only emits an error and no items.

```javascript
import { _throw } from 'rxjs/observable/throw';

const observable = _throw(new Error("Woops"));
observable.subscribe(
  x => console.log(x),
  e => console.error(e) // Error: Woops
);
```

Since `throw` is reserved in JavaScript, RxJS uses `_throw`. RxJS 6 has introduced **throwError** now!

#### range

`range` creates an Observable that emits a sequence of numbers between a range. We provide the start integer and the number of sequential integers to generate.

```javascript
import { range } from 'rxjs/observable/range';

const numbers = range(1, 10);
numbers.subscribe(x => console.log(x));
```

The above example starts at 1 and prints the next ten numbers (including 1). So output is 1, 2, 3, 4, 5, 6, 7, 8, 9, 10.

### Time based

#### interval

We have used `interval` in the last few blog posts. It creates an Observables that provides sequential numbers at specified time intervals.

```javascript
import { interval } from 'rxjs';
const observable = interval(1000);
observable.subscribe(
    value => console.log(value)
    // 1 2 3 4 5 6... so on till infinity
    // every value is logged 1 second apart
);
```

#### timer

`timer` produces an Observable **after** `x` amount of time. It keeps producing sequential numbers with a time gap of `y` milliseconds between each number. `x` and `y` are parameters `timer` takes in.

```javascript
import { timer } from 'rxjs/observable/timer';

const numbers = timer(3000, 1000);
numbers.subscribe(
  value => console.log(value)
);
```

1. The first parameter (3000) specifies how long to wait before emitting the first value.
2. `numbers` Observable will start emitting values starting at 0 once 3 seconds have passed.
3. After logging 0, it will wait one second and print 1 and continue printing sequential numbers with a 1-second gap between them.
4. The time gap between numbers is the second parameter.
5. Both parameters are optional.

##### Without either parameters

```javascript
import { timer } from 'rxjs/observable/timer';

const numbers = timer();
numbers.subscribe(
  value => console.log(value)
);
```

If we do not specify either parameter (like above), the Observable will not wait to print the first value (0). Timer Observable will only print one value and immediately complete due to absence of time gap (second parameter).

##### Without interval

```javascript
import { timer } from 'rxjs/observable/timer';

const numbers = timer(3000);
numbers.subscribe(
  value => console.log(value)
);
```

The behavior of this Observable will be the same. Only one value (0) printed and then complete. The only difference is that the first value is printed after a wait of 3 seconds.

> The difference between interval and timer is that we can specify the start time for timer.

### Additional

#### empty

I see `empty` as somewhat of an opposite to `throw` or `throwError`. `empty` creates an Observable that generates no value (like `throw`). Still, it emits a complete notification (unlike `throw` that emits an error event).

```javascript
import { empty } from 'rxjs/observable/empty';

empty().subscribe(
  x => console.log(x),
  e => console.log(e),
  () => console.log('complete')
)
```

The only output is `complete`.

`empty` has been deprecated in favor of using `EMPTY` constant.

#### never

`never` has been deprecated in favor of `NEVER` constant. `NEVER` is an Observable that emits no items and **never completes**.

```javascript
import { never } from 'rxjs/observable/never';

function logger() {
  console.log('never called');
  // never ouputs as logger is never called
}

never().subscribe(
  x => console.log(x),
  logger,
  logger
)
```

#### defer

`defer` is slightly tricky. It creates an Observable only when the Observer subscribes.
It also creates a new Observable for each Observer.
The new Observable is generated using an Observable factory function.
Every Observer subscribed to the Observable might think they are subscribed to the same Observable, but they are subscribing to their own Observable.

It delays the Observable creation until subscription. To illustrate the working of `defer` and showcase the difference between `defer` and `of`, let's take an example.

```javascript
import { of } from 'rxjs/observable/of';
import { defer } from 'rxjs/observable/defer';

const observable1 = of(new Date()); // will capture current date time
const observable2 = defer(() => of(new Date())); // will capture date time at the moment of subscription

console.log(new Date()); // 2020-07-06T06:19:25.368Z

observable1.subscribe(
  date => console.log(date) // 2020-07-06T06:19:25.368Z
)

observable2.subscribe(
  date => console.log(date) // 2020-07-06T06:19:25.369Z
)
```
