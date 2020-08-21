---
layout: post
title:  "AggregateError: Combining Multiple Errors"
date:   2020-08-20 12:00:00 -0500
---

AggregateError object in JavaScript takes multiple iterable `Error` and combines them into a single error. One of the major use cases is when multiple errors need to be reported by an operation. `Promise.any()` uses it when all promises are rejected.

`AggregateError()` is the constructor, and the object has two properties on the prototype: a message and a name. The name defaults to `AggregateError`, and the default message is an empty string.

### Creating an AggregateError

`AggregateError` has the following syntax:

```javascript
AggregateError(Array<Error>, "message");
```

Let's take an example.

```javascript
try {
    throw new AggregateError([
        new Error("Error in Login Handler"),
    ], "Access Denied");
} catch (e) {
    console.log(e instanceof AggregateError); // true
    console.log(e.message); // Access Denied
    console.log(e.name); // "AggregateError"
    console.log(e.errors); // [ Error: "Error in Login Handler" ]
}
```

🚨Be advised that not every browser supports AggregateError. You might need a polyfill like `es-aggregate-error`. Currently, there is no support for Opera, IE, Edge or Node!

If you do use a polyfill you can do something like below:

```javascript
const AggregateError = require('es-aggregate-error');

try {
    throw new AggregateError([
        new Error("Error in Login Handler"),
    ], "Access Denied");
} catch (e) {
    console.log(e instanceof AggregateError); // true
    console.log(e.message); // Access Denied
    console.log(e.name); // "AggregateError"
    console.log(e.errors); // [ Error: "Error in Login Handler" ]
}
```

### Using it with Promise.any or Promise.all

```javascript
Promise.any([
    Promise.reject(new Error("Promise Failed")),
]).catch(e => {
    console.log(e instanceof AggregateError); // true
    console.log(e.errors); // [ Error: "Promise Failed" ]
});
```
