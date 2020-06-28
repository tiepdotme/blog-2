---
layout: post
title: "Bind, Call & Apply"
date: 2020-06-26 12:00:00 -0500
---

### Bind

The `this` keyword plays a vital role in JavaScript. In JavaScript, this is based on how a function was called and not where it was declared (arrow functions behave the other way around).

Let's take an example to demonstrate the `this` keyword.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function() {
        return `Hello, ${this.name}`;
    }
}

console.log(sayGreeting.hello()); // Hello, Parwinder
```

The `hello` method can access the `name` property of the object `sayGreeting`. When I ran the method, it is prefixed by `sayGreeting.` and hence it runs in the context of `sayGreeting` object.

Instead if I did this:

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function() {
        return `Hello, ${this.name}`;
    }
}

const hello = sayGreeting.hello;

console.log(hello === sayGreeting.hello); // true
console.log(hello()); // Hello, undefined
```

Even though the variable `hello` is equal to the method on `sayGreeting`, the variable is not executed in the context of `sayGreeting`. It is executed in the `window` or global context.

`bind` allows us to **bind** context. It creates a new function where the `this` keyword is set to what we pass to `bind` method.

To make the above example, I can use the `bind` method to bind the context of `sayGreeting` to `hello` variable.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function() {
        return `Hello, ${this.name}`;
    }
}

const hello = sayGreeting.hello.bind(sayGreeting);
console.log(hello()); // Hello, Parwinder
```

#### Where do we need to bind in real life?

In all the above example, the data being accessed and the function trying to access it is in the same object. There are times when you want to **borrow** method from an object but run it in the context of another.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function () {
        return `Hello, ${this.name}`;
    }
}

const nameObject = {
    name: "Lauren"
}

const hello = sayGreeting.hello.bind(nameObject);

console.log(hello()); // Hello, Lauren
```

I have the `hello` method in `sayGreeting` object. There is no need to recreate it in `nameObject`. I can borrow the `hello` method and run it in the context of `nameObject`.

### Call

`call()` and `apply()` differs from `bind()`. `bind()` returns a new function whereas `call()` and `apply()` invoke the existing function immediately. `call()` takes `this` as the first argument and then it allows you to pass arguments one by one. These arguments would be passed to the function we called.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function () {
        return `Hello, ${this.name}`;
    }
}

console.log(sayGreeting.hello.call(sayGreeting)); // Hello, Parwinder
```

With arguments:

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function (trait, color) {
        return `Hello, ${this.name}. I see you ${trait} ${color}. It is my favorite too!`;
    }
}

console.log(sayGreeting.hello.call(sayGreeting, "like", "red"));
// Hello, Parwinder. I see you like red. It is my favorite too!
```

### Apply

`apply()` even though executes the function immediately like `call()` does but it takes an array of arguments as a second parameter instead of comma-separated values.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function () {
        return `Hello, ${this.name}`;
    }
}

console.log(sayGreeting.hello.apply(sayGreeting)); // Hello, Parwinder
```

No difference between `apply` and `call` when done without arguments. But, when used with arguments.

```javascript
const sayGreeting = {
    name: "Parwinder",
    hello: function (trait, color) {
        return `Hello, ${this.name}. I see you ${trait} ${color}. It is my favorite too!`;
    }
}

console.log(sayGreeting.hello.apply(sayGreeting, ["like", "red"]));
// Hello, Parwinder. I see you like red. It is my favorite too!
```

`apply` makes it easier to send n number of arguments in an array. Sending multiple arguments is easier now in ES6 by using the rest (...) operator.
