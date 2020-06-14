---
layout: post
title:  "this Keyword"
date:   2020-06-12 12:00:00 -0500
---

## `this` keyword and functions

The `this` keyword in JavaScript is a property of an execution context, wether it is global, function, or eval. For regular JavaScript functions, `this` could change based upon how they were called.

1. By itself, `this` refers to the global object.
2. The new object, when used in a constructor.
3. The base object, when the function enclosing, was called as a method of the object.
4. `undefined` in case of **strict mode**.
5. In an event, `this` refers to the element that received the event.

We have used this behavior for so long that most JavaScript developers are used to it. Let's go over some examples:

1. By itself, `this` refers to the global object.

```javascript
function foo() {
    return this;
};

console.log(foo()); // window object in a browser, global object for node execution
```

2. The new object, when used in a constructor.

```javascript
function Order(main, side, dessert) {
    this.main = main;
    this.side = side;
    this.dessert = dessert;
    this.order = function () {
        return `I will have ${this.main} with ${this.side} and finish off with a ${this.dessert}`;
    }
}
const newOrder = new Order("sushi", "soup", "yogurt");

console.log(newOrder.order());
// I will have sushi with soup and finish off with a yogurt
```

3. The base object, when the function enclosing, was called as a method of the object

```javascript
const myObject = {
    main: "butter chicken",
    side: "rice",
    dessert: "ice cream",
    order: function () {
        return `I will have ${this.main} with ${this.side} and finish off with ${this.dessert}`;
    }
}

console.log(myObject.order());
// I will have butter chicken with rice and finish off with ice cream
```

In the above example, `this` refers to `myObject,` and it can access the properties of the object.

4. `undefined` in case of **strict mode**

```javascript
"use strict";
function foo() {
    return this;
};

console.log(foo() === undefined);
```

5. In an event, `this` refers to the element that received the event.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta content="text/html;charset=utf-8" http-equiv="Content-Type">
    <meta content="utf-8" http-equiv="encoding">
</head>
<body>
    <button id="mybutton">
        Click me!
    </button>
    <script>
        var element = document.querySelector("#mybutton");
        element.addEventListener('click', function (event) {
            console.log(this); // Refers to the button clicked
            console.log(this.id); // mybutton
        }, false);
    </script>
</body>
</html>
```

We discussed arrow functions in the previous blog post and `this` keyword in the current one. The next blog post will combine arrow functions with `this` keyword to showcase how arrow functions behave differently than standard function expressions.
