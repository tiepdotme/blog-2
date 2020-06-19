---
layout: post
title:  "Timers & Interval"
date:   2020-06-18 12:00:00 -0500
---

In JavaScript, if you would like to run a piece of code after a certain amount of time, you need a timer. If you would instead repeatedly run the code after fixed periods, you need an interval.

### Timer

Timers are done using `setTimeout`.

```javascript
setTimeout(() => {
    console.log("hello");
}, 2000);
```

The first argument is always a function or piece of code to execute. In this case, we are logging "hello" to the console. The second argument is the duration of the timer in milliseconds. We will be printing "hello" after a 2 second (pr 2000 ms) delay.

We can also pass n number of parameters to `setTimeout` after the wait duration. These parameters are passed as arguments to the function which will be executed.

### Intervals

Intervals are done using `setInterval`.

```javascript
setInterval(() => {
    console.log("hello");
}, 2000);
```

Same syntax for `setInterval` as for `setTimeout`. In `setInterval`, we will be printing "hello" to the console **every 2 seconds**. The code will keep printing the string until we clear the interval.

### Stopping/Clearing timers and intervals

Both `setTimeout` and `setInterval` return a unique timer/interval ID. If we save this ID in a variable, we can use it to clear/stop the timer/interval.

To clear timer, use `clearTimeout` and to clear interval use `clearInterval`.

```javascript
const intervalId = setInterval(() => {
    console.log("hello");
}, 2000);

clearInterval(intervalId);
```
