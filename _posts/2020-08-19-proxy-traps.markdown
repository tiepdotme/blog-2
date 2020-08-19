---
layout: post
title:  "Proxy Traps"
date:   2020-08-19 12:00:00 -0500
---

We went over `Proxy` and how it handles or intercepts incoming operations to a target object in [the last blog post](https://bhagat.me/blog/2020/08/18/proxy-object.html). Handler functions are called traps because they **trap** calls to the target object.

Thirteen internals traps were introduced in ES2015.

| Internal Method       | Handler Method           |
|-----------------------|--------------------------|
| [[GetPrototypeOf]]    | getPrototypeOf           |
| [[SetPrototypeOf]]    | setPrototypeOf           |
| [[IsExtensible]]      | isExtensible             |
| [[PreventExtensions]] | preventExtensions        |
| [[GetOwnProperty]]    | getOwnPropertyDescriptor |
| [[DefineOwnProperty]] | defineProperty           |
| [[HasProperty]]       | has                      |
| [[Get]]               | get                      |
| [[Set]]               | set                      |
| [[Delete]]            | deleteProperty           |
| [[OwnPropertyKeys]]   | ownKeys                  |
| [[Call]]              | call                     |
| [[Construct]]         | construct                |

You can read more and see the [list here](https://www.ecma-international.org/ecma-262/9.0/#table-30).

We will not go over all of them but a few that you would use frequently.

### Get, Set & Delete

Get, Set and Delete are relatively straightforward. They allow you to read, write and remove from the target object.

```javascript
const target = {
    id: 1,
    name: "John Doe",
    age: 42,
    income: 70000,
    hobby: "Dancing"
};

const handler = {
    get: function (target, property) {
        if (["age", "income"].includes(property)) {
            return `Access Denied, ${property} is restricted.`;
        }
        return Reflect.get(...arguments);
    },
    set: function (target, property, value) {
        if (property === "name") {
            target[property] = `${value} Doe`;
        } else {
            target[property] = value;
        }
        return true; // set and delete should return true or false based on the success
    },
    deleteProperty: function (target, property, value) {
        if (property === "id") {
            throw new Error("Required Property. Cannot Delete.");
        }
        delete target[property];
        return true; // set and delete should return true or false based on the success
    }
};

const proxy = new Proxy(target, handler);

// Get
console.log(proxy.name); // John Doe
console.log(proxy.age); // Access Denied, age is restricted.
console.log(proxy.income); // Access Denied, income is restricted.
console.log(proxy.hobby); // Dancing

// Set
proxy.name = "Jane"
proxy.age = 40;
console.log(proxy.name); // Jane Does
console.log(proxy.age); // Access Denied, age is restricted.
console.log(target.age); // 40

// Delete
console.log(delete proxy.id); // Required Property. Cannot Delete.
console.log(proxy.id); // 1
console.log(delete proxy.hobby); // true
console.log(proxy.hobby); // undefined
```

### ownKeys

`Object.keys` or `for..in` loops are intercepted by `ownKeys`.

```javascript
const target = {
    id: 1,
    name: "John Doe",
    age: 42,
    income: 70000,
    hobby: "Dancing"
};

const handler = {
    ownKeys: (target, property, value) => {
        return Object.keys(target); // returns all keys from object
    }
};

const proxy = new Proxy(target, handler);

for (let key in proxy) {
    console.log(key); // id, name, age, income, hobby
}
```

We can use `filter` to filter out values (like sensitive information).

```javascript
const target = {
    id: 1,
    name: "John Doe",
    age: 42,
    income: 70000,
    hobby: "Dancing"
};

const handler = {
    ownKeys: (target, property, value) => {
        return Object.keys(target).filter((value) => !["age", "income"].includes(value));
    }
};

const proxy = new Proxy(target, handler);

for (let key in proxy) {
    console.log(key); // id, name, hobby
}
```

### has

The `has` trap intercepts `in` calls.

```javascript
const budget = {
    "minimum": 500,
    "maximum": 10000
};

const handler = {
    has(target, property) {
        return property >= budget.minimum && property <= budget.maximum;
    }
};

const expenseRange = new Proxy(budget, handler);

console.log(5000 in expenseRange); // true
console.log(10001 in expenseRange); // false
```
