---
layout: post
title:  "Arrow Functions & this keyword"
date:   2020-06-13 12:00:00 -0500
---

We learned about [arrow functions](http://bhagat.me/blog/2020/06/11/arrow-functions.html) and the [`this` keyword](http://bhagat.me/blog/2020/06/12/this-keyword.html) in previous blog posts. Now we will combine the two and see how arrow functions behave differently compared to standard function expressions.

Arrow functions, for the most part, work like function expressions with a concise syntax. The critical difference is that arrow functions do not have their bindings to `this` keyword.

For a function expression, `this` changes based on the context a function is called in. For arrow functions, `this` is based on the enclosing lexical [scope](http://bhagat.me/blog/2020/05/28/scope-in-javascript.html). Arrow functions follow normal variable lookup. They look for `this` in the current scope, and if not found, they move to the enclosing scope.

We will use the same scenarios as the previous blog post.

1. By itself.
2. When used in a constructor.
3. Called as a method of the object.
4. In the case of **strict mode**.
5. In an event.

Let us use arrow functions in the above five examples and see how they behave:

### By itself

```javascript
const foo = () => {
    return this;
}

console.log(foo()); // window or global object
```

**Exactly same as function expressions.**

<br/>

### When used in a constructor.

```javascript
const Order = (main, side, dessert) => {
    this.main = main;
    this.side = side;
    this.dessert = dessert;
    this.order = function () {
        return `I will have ${this.main} with ${this.side} and finish off with a ${this.dessert}`;
    }
}
const newOrder = new Order("sushi", "soup", "yogurt"); // Order is not a constructor

console.log(newOrder.order());
```

**Arrow functions cannot be used as constructors**. They differ in this case. Although changing the `this.order` to an arrow function would work the same if we did not use the arrow function as a constructor.

```javascript
function Order(main, side, dessert) {
    this.main = main;
    this.side = side;
    this.dessert = dessert;
    this.order = () => {
        return `I will have ${ this.main } with ${ this.side } and finish off with a ${ this.dessert } `;
    }
}
const newOrder = new Order("sushi", "soup", "yogurt");

console.log(newOrder.order());
// I will have sushi with soup and finish off with a yogurt
```
<br/>
### Called as a method of the object.

```javascript
const myObject = {
    main: "butter chicken",
    side: "rice",
    dessert: "ice cream",
    order: () => {
        return `I will have ${this.main} with ${this.side} and finish off with ${this.dessert}`;
    }
}

console.log(myObject.order());
// I will have undefined with undefined and finish off with undefined
```

**Does not work at all like function expressions!**. Arrow functions are not direct replacements of function expressions when used as methods of an object.

Why did it provide `undefined` for the variables **main, side, and dessert**? The `this` within the arrow function is the one that was current where we defined the object `myObject` (in this example window). The window does not have the three variables `order` is looking for.

<br/>
### In the case of **strict mode**.

```javascript
"use strict";
const foo = () => {
    return this;
};

console.log(foo() === undefined); // false
console.log(foo()); // window or global object
```

**Does not work at all like function expressions!** because the rules of lexical `this` take precedence over strict-mode `this` rules.

<br/>
### In an event.

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
        element.addEventListener('click', (event) => {
            console.log(this); // window object
            console.log(this.id); // undefined
        }, false);
    </script>
</body>
</html>
```

**Does not work at all like function expressions!** The value of `this` inside an arrow function is determined by where the arrow function is defined, not where it is used. In this example, we can access the element using `event.currentTarget.` We can read more [here](https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget).

Summary: Except when using it on its own, arrow function behaves differently compared to function expressions. They are concise and provide advantages but do know when not to use them as a direct replacement of function expressions.
