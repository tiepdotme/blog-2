---
layout: post
title:  "Closures in JavaScript"
image: "/assets/social/closures.png"
description: "Closures in JavaScript is a fundamental pillar. They combine lexical scoping, function execution, and garbage collection."
date:   2020-05-30 16:00:00 -0500
---

## What are closures?

I consider closures in JavaScript as an advanced topic. It is one of the topics that gets asked in interviews a lot.
Understanding closures would be more comfortable if you have read my previous blog posts or know about scope in JavaScript.

Function scope in JavaScript means that a variable declared inside a function is only accessible in that function. It is, however, accessible to the function and any of its child functions. Closures take this one step further. Closure makes sure that the parent scope is available to the child function even after the parent function has finished execution.

### Example

```javascript
function outer() {
    const outerVariable = "outer";
    function inner() {
        const innerVariable = "inner";
        console.log(`${outerVariable} ${innerVariable}`); // outer inner
    }
    inner();
}

outer();
```

I created and executed the `outer` function above. This action creates and invokes `inner` function. `inner` function successfully logs the variable it has declared and the variable in the parent function. It is expected since the child function has access to the parent function's scope.

Now let us not call the `inner` function and instead return it.

```javascript
function outer() {
    const outerVariable = "outer";
    function inner() {
        const innerVariable = "inner";
        return (`${outerVariable} ${innerVariable}`);
    }
    return inner;
}

const returnFunction = outer();
console.log(returnFunction); // Function returnFunction
```

Variable `returnFunction` is a function because that is what was returned by `outer`. No surprises.

🚨**At this point, execution of `outer` function has finished, and the return value has been assigned to a new variable**.

This is key. JavaScript garbage collection should have removed all traces of `outerVariable` because that is what happens when a function is popped off the stack and finished executing. Let us run `returnFunction`.

```javascript
function outer() {
    const outerVariable = "outer";
    function inner() {
        const innerVariable = "inner";
        return (`${outerVariable} ${innerVariable}`); // outer inner
    }
    return inner;
}

const returnFunction = outer();
console.log(returnFunction); // Function returnFunction
console.log(returnFunction()); // outer inner
```

Surprise surprise! It can still log the value of a variable in its parent function (which has finished executing).

JavaScript garbage collection does not clear out variables of a function if that function has child functions returned. These child functions can run later and are fully eligible to access the parent's scope by the principal of lexical scoping.

This behavior of garbage collection is not only limited to child function. A variable is not garbage collected as long as **anything** maintains a reference to it.

### Real World Example

Let's say that I am programming about a car. This car can accelerate like a real-world car, and whenever it does accelerate, the speed of the car increases.

```javascript
function carMonitor() {
    var speed = 0;

    return {
        accelerate: function () {
            return speed++;
        }
    }
}

var car = new carMonitor();
console.log(car.accelerate()); // 0
console.log(car.accelerate()); // 1
console.log(car.accelerate()); // 2
console.log(car.accelerate()); // 3
console.log(car.accelerate()); // 4
```

You can see how the speed of the car is provided by `carMonitor,` and it is accessible by `accelerate` function. Every time I call `accelerate`, not only can it access the said variable but increment it from last value and return it.

### Creating Private Variables using Closures

Let's take the example for `carMonitor.`

```javascript
function carMonitor() {
    var speed = 0;

    return {
        accelerate: function () {
            return speed++;
        }
    }
}

var car = new carMonitor();
console.log(car.accelerate()); // 0
console.log(car.accelerate()); // 1
console.log(car.accelerate()); // 2
console.log(car.accelerate()); // 3
console.log(car.accelerate()); // 4
console.log(speed); // speed is not defined
```

You can see that the variable is a private variable for function `carMonitor`, and it is only accessible by the child function `accelerate.` No one outside can access it. One could argue that it is expected due to the function scope. It is private to `carMonitor`, and it is private to each new instance of `carMonitor`.

> Every instance maintains its copy and increments it!

This should help you realize the power of closures!

```javascript
function carMonitor() {
    var speed = 0;

    return {
        accelerate: function () {
            return speed++;
        }
    }
}

var car = new carMonitor();
var redCar = new carMonitor()
console.log(car.accelerate()); // 0
console.log(car.accelerate()); // 1
console.log(redCar.accelerate()); // 0
console.log(redCar.accelerate()); // 1
console.log(car.accelerate()); // 2
console.log(redCar.accelerate()); // 2
console.log(speed); // speed is not defined
```

`car` and `redCar` maintain their own private `speed` variables, and `speed` is not accessible outside. We are enforcing the consumer to use the methods defined on the function or class rather than accessing the properties directly (which they should not). This is how you would encapsulate your code.

I hope the article cleared up any doubts about closures in JavaScript!

**BONUS**

Here is an interview question about closures that is asked fairly frequently.

What do you think the output of the following piece of code will be:

```javascript
for (let i = 0; i <= 5; i++) {
    setTimeout(function () {
        console.log(i);
    }, 1000);
}
```

If you guessed numbers from 0 to 5 with a one-second gap, you are in for a surprise. By the time 1 second has passed for the setTimeout, `i` has a value of 6 at the time of invocation! We would like to use the value of `i` from time of creation and IIFE + closures will help you do that.

```javascript
for (let i = 0; i <= 5; i++) {
    (function (i) {
        setTimeout(function () {
            console.log(i);
        }, 1000);
    })(i);
}
```
