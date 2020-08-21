---
layout: post
title: "Subjects & Behavior Subject (Observables)"
image: "/assets/social/subjects.png"
date: 2020-07-05 12:00:00 -0500
---

### Subjects

Subjects are a special type of Observable. Observables are unicast, but Subjects are multicast. What does that mean? Whenever an Observer subscribes to an Observable, they own their independent execution of the Observable. That is not the case with Subjects. Subjects are like emitters that have many listeners and Subjects maintain a list of its listeners.

Another critical characteristic of Subject is that they can act as **Observable and Observer!**
- An Observer can subscribe to a Subject, and they will get values as if they are subscribed to an Observable. The Observer will not be able to tell the difference.
- The Subject also has the `next`, `error` and `complete` method on it like an Observer. Any value set to `next` will be sent (multicasted) to all Observers registered to this Subject.

The idea that a Subject can be an Observable and an Observer makes it an excellent candidate for being a proxy between Observables.

```javascript
import { Subject } from 'rxjs';

const subject = new Subject();

subject.subscribe(
  data => console.log(`observer A: ${data}`)
);

subject.subscribe(
  data => console.log(`observer B: ${data}`)
);

subject.next(10);
subject.next(20);
```

The output will be:

```console
observer A: 10
observer B: 10
observer A: 20
observer B: 20
```

In the above example, we use a Subject as an Observable with two different Observers. Both of them get the values passed to `next`. Extending this, we can also subscribe to an Observable by passing it a Subject. Then we can subscribe to that Subject. Example below:

```javascript
import { Subject } from 'rxjs';
import { from } from 'rxjs/observable/from';

const subject = new Subject();

subject.subscribe(
    (value) => console.log(`observerA: ${value}`)
);
subject.subscribe(
    (value) => console.log(`observerB: ${value}`)
);

const observable = from([10, 20, 30]); // Both subject subscribers will get the three values

observable.subscribe(subject); // You can subscribe providing a Subject
```

The output will be:

```console
observerA: 10
observerB: 10
observerA: 20
observerB: 20
observerA: 30
observerB: 30
```

### BehaviorSubject

A BehaviorSubject is a particular type of Subject. It maintains the latest (or current value) emitted to its consumers. Whenever a new Observer subscribes to it, BehaviorSubject will immediately send the latest value to the Observer.

A more real-life example of this could be payroll for an employee. The occurring monthly salary is like a Subject. The net balance now in the employee account because of these payrolls is BehaviorSubject.

```javascript
import { BehaviorSubject } from 'rxjs';
const behaviorSubject = new BehaviorSubject(500); // Initial value : 500

behaviorSubject.subscribe(
    (value) => console.log(`Observer A: ${value}`) // A will get the initial value set, 500
);

behaviorSubject.next(1000); // We set two more value 1000, 2000
behaviorSubject.next(2000); // A will receive both values

behaviorSubject.subscribe(
    (value) => console.log(`Observer B: ${value}`) // B will only receive the latest value of 2000
);

behaviorSubject.next(3000); // Since both Observers exist now
// Both will get 3000
// A gets 500, 1000, 2000, 3000
// B gets the latest at time of creation (2000) and 3000
```

So, the output will be:

```console
Observer A: 500
Observer A: 1000
Observer A: 2000
Observer B: 2000
Observer A: 3000
Observer B: 3000
```

We started with Observables and got into the slightly complicated territory of Subjects and BehaviorSubjects. The only way to learn them and understand them is through practice. Try a few real-life code bases and implement them using Observables/Subjects.

Happy coding 👋🏼
