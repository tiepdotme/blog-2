---
layout: post
title:  "Proxy Object"
date:   2020-08-18 12:00:00 -0500
---

`Proxy` object wraps a target object and intercepts incoming operations to the target object using a handler object. `Proxy` was introduced in ES2015, and it is not as popular as other critical features of ES6.

A Proxy has two parameters:

- `target`: the original object to proxy.
- `handler`: an object that defines how intercepted operations will be handled.

Here is an example to explain:

```javascript
const target = {
    name: "John Doe",
    age: 42
};

const handler = {};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John Doe
console.log(proxy.age); // 42
```

Our `target` consists of name and age. Since our `handler` is empty, when we ask for the name and age, it forwards the name and age from `target`.

Let's add some interception rules to the handler.

```javascript
const target = {
    name: "John Doe",
    age: 42
};

const handler = {
    get: () => { // get trap
        return "Access Denied";
    }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // Access Denied
console.log(proxy.age); // Access Denied
```

You can see how the behavior of our console messages has changed now. When we try "getting" the name and age now, the action gets intercepted, and we return an "Access Denied" message.

The `get()` trap is fired when a property of the target object is accessed via the proxy object. We have a similar trap for `set()` and `apply()` (and a couple more).

We can add more behavior to it.

```javascript
const target = {
    name: "John Doe",
    age: 42,
    income: 70000,
    hobby: "Dancing"
};

const handler = {
    get: function (target, value) {
        if (["age", "income"].includes(value)) {
            return `Access Denied, ${value} is restricted.`;
        }
        return Reflect.get(...arguments);
    },
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John Doe
console.log(proxy.age); // Access Denied, age is restricted.
console.log(proxy.income); // Access Denied, income is restricted.
console.log(proxy.hobby); // Dancing
```

We check if the user has asked for age or income and deny the request. If we are asked for public information like name and hobby, we pass that on.

`Reflect` provides accessors to the original behavior and allows us to redefine some.

We will discuss `Reflect` class and **Traps** in a future blog post.
