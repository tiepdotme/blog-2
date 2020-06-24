---
layout: post
title: "New Promise Methods: allSettled & any"
date: 2020-06-22 12:00:00 -0500
---

## Introduction

We covered basic and advanced promises in the last two blog posts. There are two reasonably new operators/methods to promises that can make life easier. Let's go over them.

### AllSettled

ES2020 or ES11 introduced `promise.allSettled` so it is **fairly new and should be used with caution**. Check the browsers you are planning to support.

`allSettled` returns a promise when all the promises provided to it have either resolved or rejects. The return is an array of objects where each object describes the outcome of input promises.

`allSettled` and `promise.all` have a minor difference.

`promise.all` rejects with the first rejection of any of the promises given as input. So if we provide five promises to `promise.all` and two of them fail, `promise.all` will reject with the result of the very first failure.

`promise.allSettled` on the other hand will wait for all promises to finish and provide the result of each promise provided as input (be it resolved or rejected). Use `promise.allSettled` when the async promises are not dependent on each other, and you can retry only the ones that failed. If your course of action depends on all async tasks completing successfully before moving on, use `promise.all`.

```javascript
const promise1 = Promise.resolve("Parwinder");
const promise2 = new Promise((resolve) => {
    setTimeout(() => {
        resolve("Lauren");
    }, 2000);
});
const promise3 = Promise.reject("Robert");
const promise4 = Promise.resolve("Eliu");

Promise.allSettled([promise1, promise2, promise3, promise4]).then((data) => {
    console.log(data);
});
```

Once all four promises above resolve/reject, `allSettled` will pass the result to the callback in `then`. The log will output:

```javascript
[{
  status: "fulfilled",
  value: "Parwinder"
}, {
  status: "fulfilled",
  value: "Lauren"
}, {
  reason: "Robert",
  status: "rejected"
}, {
  status: "fulfilled",
  value: "Eliu"
}]
```

### Any

🚨 `Promise.any` is currently in stage 3 of the [TC39 proposal](https://github.com/tc39/proposal-promise-any) (candidate stage). While it will most likely make it to the next release of ECMAScript, there are no guarantees. Please use it with extra caution.

`Promise.any` works with an iterable of promises. It returns a single promise that resolves with the value from the first successful promise in the iterable. If none of the promises in the iterable succeeds, it returns an `AggregateError` (a new subclass of `Error`). `AggregateError` is used to group individual errors from all promises input.

`Promise.any` is the exact opposite of `Promise.all`

```javascript
const promise1 = Promise.resolve("Parwinder");
const promise2 = new Promise((resolve) => {
    setTimeout(() => {
        resolve("Lauren");
    }, 2000);
});
const promise3 = Promise.reject("Robert");
const promise4 = Promise.resolve("Eliu");

Promise.any([promise1, promise2, promise3, promise4]).then((data) => {
    console.log(data); // Parwinder (first successful promise)
});
```

In case of rejection from all promises

```javascript
const promise1 = Promise.reject("Parwinder");
const promise2 = new Promise((resolve) => {
    setTimeout(() => {
        reject("Lauren");
    }, 2000);
});
const promise3 = Promise.reject("Robert");
const promise4 = Promise.reject("Eliu");

Promise.any([promise1, promise2, promise3, promise4]).then((data) => {
    console.log(data); // "AggregateError: No Promise in Promise.any was resolved"
});
```
