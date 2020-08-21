---
layout: post
title:  "Reflect Class"
date:   2020-08-20 12:00:00 -0500
---

### Introduction

By now, you should be aware of [Proxy](https://bhagat.me/blog/2020/08/18/proxy-object.html) and [Proxy Traps](https://bhagat.me/blog/2020/08/19/proxy-traps.html). We will discuss the `Reflect` class in this blog post.

`Reflect` helps with forwarding default operations from the handler to the target. `Reflect` methods are wrappers around an object's internal methods. The wrapper is fairly minimal and does not provide additional functionality.

### Example:

```javascript
let target = {};
Reflect.set(target, "name", "John Doe"); // target object, property and value
console.log(target.name); // John Doe
```

### Reflect & Proxy Traps

Reflect class contains methods with the same name and arguments as Proxy traps. In the above example, we used `set`, and we saw the usage of `set` as a trap in the last two blog posts.

Let's combine Proxy traps and Reflect methods.

```javascript
const target = {
    name: "John Doe",
};

const handler = {
    get(target, property) {
        return Reflect.get(target, property);
    },
    set(target, property, value) {
        return Reflect.set(target, property, value);
    }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John Doe
console.log(target.name); // John Doe

proxy.name = "Jane Doe";

console.log(proxy.name); // Jane Doe
console.log(target.name); // Jane Doe
```

All of this is good and makes sense so far, but why introduce `Reflect` at all when Proxy traps can provide access to object internal methods?

### Why Use Reflect

Let's say we have the above examples using traps.

```javascript
const target = {
    name: "John Doe",
};

const handler = {
    get(target, property) {
        return target[property];
    },
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John Doe
```

The above example is relatively simple. I want to inherit from the target object and make a `user` object.

```javascript
const target = {
    firstName: "John",
    get name() {
        return this.firstName;
    }
};

const handler = {
    get(target, property) {
        return target[property];
    },
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John

const user = {
    __proto__: proxy,
    firstName: "Jane"
};

console.log(user.name); // John, WHAT! 😲
```

When I type `user.name` I expect it to be `Jane`. `user` is inherited from a proxy to target and target has a `name` getter that returns `firstName`. So why did it not return `firstName` set in the user?

- When I asked for `user.name`, JavaScript checks the prototype chain to search for `name`.
- It goes to the parent object `proxy` and proxy has a trap that gets targets `firstName`.
- So even though we have set the `firstName` in `user` object due to inheritance from a proxy and that proxy using a trap makes it impossible to reach user's first name with the getter.

How do we fix this? Pass the correct context (`this`) to the getter in the target using `Reflect.get`.

```javascript
const target = {
    firstName: "John",
    get name() {
        return this.firstName;
    }
};

const handler = {
    get(target, property, receiver) {
        return Reflect.get(target, property, receiver);
    },
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // John

const user = {
    __proto__: proxy,
    firstName: "Jane"
};

console.log(user.name); // Jane
```

Now we see the output as expected.

`receiver` is the keyword here. It keeps a reference to `this` and passes it to the getter using `Reflect.get`.

I hope the three-part posts on Proxy and Reflect helps you understand it better.

Happy coding 👋🏽
