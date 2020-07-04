---
layout: post
title: "Creating an Operator (Observables)"
date: 2020-07-03 12:00:00 -0500
---

Operators are the foundation block of RxJS library and Observables. It gives us the power to do complex operations by using a few keywords (functions). Operators are nothing but functions that take a source Observable, perform an action on it and return a new Observable.

The `pipe` operator as we learned in the previous blog post, allows us to chain operators. Chaining is solely possible because every operator takes in an Observable and returns an Observable. The returned Observable goes as input to the next operator.

### Creating an Operator (that doesn't do anything)

Let's start by creating a simple function that takes in an Observable and returns the **same** Observable. It would not achieve anything, but it will be a start in creating Observable operators.

```javascript
import { interval } from 'rxjs/observable/interval';

function fancyOperator(source) {
    return source;
}

interval(1000).pipe(
    fancyOperator
).subscribe(
    value => console.log(value) // 0 1 2 3 4 5 6 7 8 9 .... so on with each number 1 second apart
)
```

In the above example, `fancyOperator` is taking `interval` Observable and returning the same Observable back. The subscription is happening on `fancyOperator`. There is a chain.

`interval -> fancyOperator -> subscribe`

### Adding functionality to Operator

We will add minor functionality to `fancyOperator` for learning. It will also prove that these Observables are chained precisely as specified above.

```javascript
import { interval } from 'rxjs/observable/interval';
import Rx from 'rxjs';

function fancyOperator(source) {
    return new Rx.Observable(observer => {
        observer.next("Parwinder"); // We return string Parwinder, not the same Observable anymore
        observer.complete(); // Only one value is returned and then this Observable completes
    });
}

interval(1000).pipe(
    fancyOperator
).subscribe(
    value => console.log(value) // Parwinder
)
```

Works as we expected it to 🙌🏼

### Creating a custom operator

Now we come to the meat and potatoes of the blog post, creating an operator that does something meaningful. We will create an operator that filters keyboard event and provides you data when a specific key is hit.

```typescript
import { filter } from 'rxjs/operators';
import { fromEvent } from 'rxjs/observable/fromEvent';

function filterKey(key) {
    return filter((event: KeyboardEvent) => event.key === key);
}

fromEvent(document, 'keyup')
    .pipe(
        filterKey('Enter')
    ).subscribe(
        data => console.log(data) // KeyboardEvent
    );
```

We have killed two birds with one stone in the above example. We have created a custom operator `filterKey` that filters only the key that is passed to it (Enter in this case). We have also designed it by using an operator that already exists `filter`.

`filter` returns an Observable when the keyboard event key matches the key asked for in the code.

`fromEvent` allows us to check for events, in this case on the **document** in the browser. User can hit as many keys as they want but as soon as the hit "Enter", the KeyboardEvent gets logged.

### Create an operator from scratch

We are now going to create an operator entirely from scratch, without using any help from existing operators. We are going to create a power operator that raises the number(s) to the exponential power provided.

```javascript
import Rx from "rxjs";
import { from } from "rxjs/observable/from";

const power = (num) => (source) => {
    return new Rx.Observable(observer => {
        return source.subscribe({
            next(x) {
                observer.next(
                    Math.pow(x, num)
                );
            },
            error(error) { observer.error(error); },
            complete() { observer.complete(); }
        });
    })
};

from([7, 2, 5]).pipe( // from creates an Observable from passed array
    power(2) // invoke custom operator "power" on the array Observable
).subscribe(
    data => console.log(data) // Log the sqaures of array values. 49, 4, 25
)
```

Hope this has helped you understand how operators work and how you can create some for your custom use case.

Happy coding 👋🏼
