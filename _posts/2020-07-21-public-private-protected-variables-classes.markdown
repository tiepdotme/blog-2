---
layout: post
title: "Classes: Public, Private and Protected"
image: "/assets/social/classes-members.png"
date: 2020-07-21 12:00:00 -0500
---

Even though ES6 introduced the `class` keyword that fairly well mimics classes and allows us to jump into object-oriented programming, JavaScript is missing the ability to create public, private and protected members in a class.

If you have worked with any object-oriented language, you must know the importance of internal vs external interface. Internal interface refers to the methods and properties of a class that is only accessible by the class itself and not from outside. In contrast, the external interface has methods and properties that are also accessible from outside the class.

The three major keywords at play are public, protected and private.

1. Public: These members of the class and available to everyone that can access the (owner) class instance.
2. Private: These members are only accessible within the class that instantiated the object.
3. Protected: This keyword allows a little more access than private members but a lot less than public. A protected member is accessible within the class (similar to private) and any object that inherits from it. A protected value is shared across all layers of the prototype chain. It is not accessible by anybody else.

**The protected keyword is the hardest keyword of the three to imitate in JavaScript.**

### Public

This is the default nature of JavaScript. If something has access to an object, it does have access to its members. Example:

```javascript
const myObject = {
    name: "Parwinder",
    sayMyName: function () {
        return this.name;
    }
}

console.log(myObject.name); // Parwinder
console.log(myObject.sayMyName()); // Parwinder
```

In the above example, I can access the property and method without any issue. If you would rather prefer it in a class syntax:

```javascript
class ObjectCreator {
    name;

    constructor(name) {
        this.name = name;
    }

    sayMyName() {
        return this.name;
    }
}

const myObject = new ObjectCreator("Parwinder");
console.log(myObject.name); // Parwinder
console.log(myObject.sayMyName()); // Parwinder
```

### Private

There are multiple ways of creating private variables in JavaScript. First is closures.


```javascript
function carMonitor() {
    var speed = 0;

    return {
        accelerate: function () {
            return speed++;
        }
    }
}

var car = new carMonitor();
var redCar = new carMonitor()
console.log(car.accelerate()); // 0
console.log(car.accelerate()); // 1
console.log(redCar.accelerate()); // 0
console.log(redCar.accelerate()); // 1
console.log(car.accelerate()); // 2
console.log(redCar.accelerate()); // 2
console.log(speed); // speed is not defined
```

`car` and `redCar` maintain their own private `speed` variables, and `speed` is not accessible outside. We are enforcing the consumer to use the methods defined on the function or class rather than accessing the properties directly (which they should not). This is how you would encapsulate your code.

The second way is by using the `#` notation.

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

The language enforces the encapsulation. It is a syntax error to refer to `#` names from out of scope. Public and private fields do not conflict. We can have both private #meaningOfLife and public meaningOfLife fields in the same class.

🚨 The `#` method for declaring private members of a class is in part of ES2019/ES10.

### Protected

Like I said at the beginning of this post, protected is the hardest of all 3 to implement in JavaScript. The only way that I can think of doing this is by using a class that has a getter for a property without a setter. The property will be read-only, and any object will inherit it from the class, but it will only be change-able from within the class itself.

🙏 If you have an example of creating protected members of the class (or as close to protected as we can get), please do share!

```javascript
class NameGenerator {
    _name;

    constructor(name) {
        this._name = name;
    }

    get name() {
        return this._name;
    }
}

let nameGenerator = new NameGenerator("John");
console.log(`My name is ${nameGenerator.name}`); // My name is John
nameGenerator.name = "Jane"; // Cannot assign to 'name' because it is a read-only property.
```
