---
layout: post
title: "JavaScript Type: Symbol"
image: "/assets/social/symbol.png"
date: 2020-07-18 12:00:00 -0500
---

`symbol` is a primitive data type in JavaScript. The function `Symbol()` returns a value of type `symbol` which is always **unique**. `symbol` is used for creating identifiers (because of their unique nature) and mostly as object properties.

### Creating a symbol

```javascript
let symbol1 = Symbol();
let symbol2 = Symbol("text");
```

🚨 `Symbol()` does not have a constructor and cannot support `new Symbol()`. Doing so will result in an error.

```javascript
let symbol1 = new Symbol(); // Symbol is not a constructor
```

When we create a `symbol` by passing a string, it does not make the string as a `symbol`. It still generates a new `symbol` every time. The description is just a label that doesn’t affect anything.

```javascript
console.log(Symbol("text") === Symbol("text")); // false
```

### Using symbol as an object property

We use square bracket notation to use a `symbol` as an object property.

```javascript
let userId = Symbol("id");

let employee = {
    name: "Parwinder",
    [userId]: 727
}

console.log(employee[userId]); // 727

// above log is not similar to using the string literal "userId"
console.log(employee["userId"]); // undefined
```

### Creating hidden properties with symbol

We can create hidden properties on objects that no one can modify or overwrite.

```javascript
let car = {
    name: "BMW"
};

let hiddenField = Symbol("price");

car[hiddenField] = 70000;

console.log(car); // { name: 'BMW', [Symbol(price)]: 70000 }
console.log(car[hiddenField]); // 70000
```

Creating hidden property on an object using `symbol` could have significant advantages in the right situations.

1. The hidden property will not impact any other module, library or user using the `car` object as they won't be able to see it.
2. If someone else wants a hidden property on the object, it will not conflict with yours due to uniqueness.

Even though `symbol` might seem hidden in an object but we have `Object.getOwnPropertySymbols()` that allows us to get all symbols in an object.

```javascript
let car = {
    name: "BMW"
};

let hiddenField = Symbol("price");
let anotherHiddenField = Symbol("release");

car[hiddenField] = 70000;
car[anotherHiddenField] = "07/18/2020";

console.log(Object.getOwnPropertySymbols(car)); // [ Symbol(price), Symbol(release) ]
```

### Gotcha: Symbols do not show up in `for..in`.

```javascript
let car = {
    name: "BMW",
    model: "Cooper",
    color: "Pearl White"
};

let hiddenField = Symbol("price");
let anotherHiddenField = Symbol("release");

car[hiddenField] = 70000;
car[anotherHiddenField] = "07/18/2020";

console.log(car[hiddenField]); // 70000
console.log(car[anotherHiddenField]); // 07/18/2020

for (let properties in car) {
    console.log(properties); // name, model, color
}
```
