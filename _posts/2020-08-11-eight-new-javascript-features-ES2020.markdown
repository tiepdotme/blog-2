---
layout: post
title:  "8 New Features in ES2020/ES11"
date:   2020-08-11 12:00:00 -0500
---

ES2020 or ES11 specs were finalized earlier this year. It introduced quite a few new features, and we will go over the eight key highlights from the new standard.

### Dynamic Import

Babel and Webpack allow us to import JS files as modules into our application conditionally. Dynamic imports are now natively supported. The feature has been taken to improve code splitting in JavaScript and request code on demand (allowing lazy loading).

Example:

Let's say you have a greeting module that takes in a name and display a greeting message for that name.

```javascript
export const greeting = (name) => console.log(`Hello ${name}`);
```

You can import this conditionally into your application.

```javascript
const time = "morning"; // this is dynamically set to the time of day, hardcoded for example

if (time === "morning") {
    const say = await import("./greeting.js");
    say.greeting("Parwinder"); // Hello Parwinder
}
```

### Private Class Variables

I have a [dedicated blog post](https://bhagat.me/blog/2020/07/21/public-private-protected-variables-classes.html) on class members where I talk about private variables and methods. Let's take an example for now:

```javascript
class ObjectCreator {
    #meaningOfLife;

    constructor(name) {
        this.#meaningOfLife = 42;
    }

    returnMeaningOfLife() {
        return this.#meaningOfLife;
    }

    #returnAMessage() {
        return "You will do great things in life";
    }
}

const myObject = new ObjectCreator("Parwinder");
console.log(myObject.returnMeaningOfLife()); // 42
console.log(myObject["#meaningOfLife"]); // undefined
console.log(myObject.#meaningOfLife); // SyntaxError
console.log(myObject.#returnAMessage); // SyntaxError
```

The language enforces the encapsulation. It is a syntax error to refer to # names from out of scope. Public and private fields do not conflict. We can have both private #meaningOfLife and public meaningOfLife fields in the same class.

### Optional Chaining

Check out [optional chaining](https://bhagat.me/blog/2020/06/16/optional-chaining.html) for the concept in detail. Accessing object properties is a common occurrence in JavaScript. Plenty of times, these properties are nested. When you access a property on an object where the object is missing, JavaScript throws an error.

The `?.` operator short circuits an object property evaluation. Instead of returning an error by keeping on evaluating, optional chaining terminates as soon as it finds the first undefined/null in the chain and returns `undefined`.

```javascript
const myObject = {
    name: "Parwinder",
    car: "Cybertruck",
    age: 42,
    computers: {
        first: {
            name: "iMac",
            year: 2017,
            spec: {
                cpu: "i7",
                ram: "16GB"
            }
        },
        second: {
            name: "MacBook Pro"
        }
    }
}

console.log(myObject.computers.first.spec.cpu); // i7
console.log(myObject.computers.second.spec.cpu); // Cannot read property 'cpu' of undefined
```

We can address the access error of `cpu` by using optional chaining.

```javascript
myObject?.computers?.second?.spec?.cpu // undefined

// Concise and easy to read code
// or

myObject.computers.second.spec?.cpu
```

### Promise.allSettled

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

I covered `allSettled` and `any` in the past. Read the [complete blog post here](https://bhagat.me/blog/2020/06/22/new-promise-methods-allsettled-any.html).

### String.prototype.matchAll()

`matchAll` is a new method on String prototype. It allows us to match a string against a regular expression. The return is an iterator of all matching results.

```javascript
const input = 'Hello Andy, Where is Beth? Emily was asking for her.';
const regex = /[A-E]/g;
const matches = input.match(regex);

console.log(matches); // [ 'A', 'B', 'E' ]
```

### globalThis

We use different syntax for accessing the global object depending on where we are executing out code. In the browser, we can use `window`, `self` or `frame`, but with Web Workers, we are limited to `self`. It is entirely different in Node where you have to use `global`.

`globalThis` aims at providing a standard way of accessing the global object.

```javascript
console.log(globalThis); // Window {...} for browsers
console.log(globalThis); // Object [global] {...} for Node
console.log(globalThis); // DedicatedWorkerGlobalScope {...} for Web Workers
```

### BigInt

`BigInt` is a numeric type to provide support for integers of arbitrary length (numbers larger than `2 ** 53 - 1` or 9007199254740991).

We can create `BigInt` by appending `n` to the end of an integer or by calling the `BigInt()`.

```javascript
const bigint = 9879846412313194464434496849n;
const bigintByMethod = BigInt("9879846412313194464434496849");

console.log(bigint); // 9879846412313194464434496849
console.log(bigintByMethod); // 9879846412313194464434496849

console.log(bigint === bigintByMethod); // true

console.log(typeof bigint); // bigint
console.log(typeof bigintByMethod); // bigint

const bigintFromExisting = BigInt(25);

console.log(bigintFromExisting); // 25
console.log(typeof bigintFromExisting); // bigint
```

### Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns its right-hand side operand when its left-hand side is `null` or `undefined`, otherwise returns the left-hand side.

```javascript
const luckyNumber = 0 ?? 42;
console.log(luckyNumber); // 0

const employeeName = null ?? "Parwinder";
console.log(employeeName); // Parwinder
```

🚨 Keep in mind that the operator does not work on `false` or `NaN`. This is where it differs from the OR `||` operator. The OR operator always returns a truthy value, whereas `??` operator always returns a non-nullish value.

If there are other new features, you would like me to cover, feel free to email me at `contact@bhagat.me`! Or leave a comment with what I might be missing.

Happy coding 👋🏼
