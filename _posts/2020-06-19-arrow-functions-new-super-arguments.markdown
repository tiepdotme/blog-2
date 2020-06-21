---
layout: post
title: "Arrow Functions: new keyword, arguments and super"
date: 2020-06-19 12:00:00 -0500
---

We learned about [arrow functions](https://bhagat.me/blog/2020/06/11/arrow-functions.html) and how it [behaves differently](https://bhagat.me/blog/2020/06/13/arrow-functions-this-keyword.html) with [`this` keyword](https://bhagat.me/blog/2020/06/12/this-keyword.html).

Arrow functions behave differently when it comes to `this` keyword. It also has no bindings to `arguments`, `new`, and `super` keyword!

### Arguments

The `arguments` object is an Array-like object that allows us to access all the arguments passed to a function.

```javascript
function addThreeNumbers(a, b, c) {
    console.log(arguments.length); // 3
    console.log(arguments[0]); // 4
    console.log(arguments[1]); // 17
    console.log(arguments[2]); // 22
    return a + b + c;
}

console.log(addThreeNumbers(4, 17, 22)); // 43
```

`arguments` for arrow functions is a reference to the arguments of the enclosing scope instead.

```javascript
const bar = x => console.log(arguments);

console.log(bar()); // Uncaught ReferenceError: arguments is not defined
```

We can solve this "problem" with a workaround. Use the `rest` operator when you need access to arguments.

```javascript
const addThreeNumbers = (...args) => {
    console.log(args.length); // 3
    console.log(args[0]); // 4
    console.log(args[1]); // 17
    console.log(args[2]); // 22
    return args[0] + args[1] + args[2];
}

console.log(addThreeNumbers(4, 17, 22)); // 43
```

You can make the above example a bit cleaner using destructuring.

```javascript
const addThreeNumbers = (...args) => {

    const [a, b, c] = args;

    console.log(args.length); // 3
    console.log(a); // 4
    console.log(b); // 17
    console.log(c); // 22

    return a + b + c;
}

console.log(addThreeNumbers(4, 17, 22)); // 43
```


### New

Arrow functions cannot be used as constructors. `new` will throw an error when used with arrow functions.

```javascript
const foo = () => { };
const bar = new foo(); // foo is not a constructor
```

Arrow functions are missing a [Construct](http://www.ecma-international.org/ecma-262/6.0/#table-6) internal method.

### Super

We cannot use the `super` keyword with arrows either per ES spec.

```javascript
class Base {
    public foo = () => {
        console.log("Hello");
    }
}

class Child extends Base {
    public bar() {
        super.foo(); // Only public and protected methods of the base class are accessible via the 'super' keyword.
    };
}
```

Instead, use regular functions in such a case.

```javascript
class Base {
    public foo() {
        console.log("Hello");
    }
}

class Child extends Base {
    public bar() {
        super.foo();
    };
}
```

### Bonus 🤑

1. Arrow functions do not have a `prototype` property.
   ```javascript
   var Foo = () => { };
   console.log(Foo.prototype); // undefined
   ```
2. Arrow functions cannot be used as generators. They do not have a `yield` keyword.
