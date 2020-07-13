---
layout: post
title:  "The New Keyword in JavaScript"
image: "/assets/social/new.png"
date:   2020-06-01 15:00:00 -0500
---

## New Keyword

We did talk about how everything in JavaScript is an object. This makes it essential to know how to create new objects or new **instances** of objects.

You would create a new object in JavaScript in two ways:

```javascript
const myObject = {
    name: "Parwinder",
    age: 33
};

console.log(myObject); // { name: 'Parwinder', age: 33 }
```

This is known as literal notation or [object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

```javascript
const myDate = new Date("06/01/2020");
console.log(myDate); // Mon Jun 01 2020 00:00:00 GMT-0500 (Central Daylight Time)
```

The second method is using the `new` keyword. Both of these ways of creating objects create a **new instance** of an `Object.` We can use the `new` keyword with anything that has a constructor.

Identical object initializers or literal notations create distinct objects that will not compare to each other as equal. Objects are created as if a call to `new Object()` was made.

### Creating an object of a custom type

You saw in the date example that I created a new instance of `Date` object. `new` keyword allows you to create a new instance of any custom object as well.

```javascript
function Car(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
}

const myCar = new Car("BMW", "X5", 2015);
const igorCar = new Car("Tesla", "Model S", 2020);
const laurenCar = new Car("Ford", "Escape", 2015);

console.log(myCar); // Car { make: 'BMW', model: 'X5', year: 2015 }
console.log(igorCar); // Car { make: 'Tesla', model: 'Model S', year: 2020 }
console.log(laurenCar); // Car { make: 'Ford', model: 'Escape', year: 2015 }

console.log(typeof myCar); // object
console.log(myCar instanceof Car); // true
```

I created three new and separate instances of the `Car` object with their unique properties of make, model, and year. Since this is an object, it is of no surprise that the `typeof` variable provides an object as output. When you do an instanceOf check, it comes back true, for instance, of a `Car` as we have instantiated these object variables from `Car` object. `Car` object acts as the **blueprint** or the **mother object** for myCar, igorCar, and laurenCar.

### What happens when I use the new keyword?

In the above example, `Car` will be called a constructor function. It helps us to construct objects with properties we have defined.

When we use the `new` keyword, it goes through four steps:

1. It creates a new empty object
2. It sets the **prototype** property of the new empty object to the constructor function's prototype property.
3. It binds the property/function declared with `this` keyword to the new object.
4. It returns the newly created object.

### Why is it important to know about the new keyword?

The `new` keyword, classes, objects, `this` property and prototype are the foundations of object-oriented programming in JavaScript. You would hear OOPs or functional programming in JavaScript. Neither is better or worse, but there are two different ideologies of how we should write code in JavaScript. Knowing about these principles can make you aware of how the two coding styles work, their advantages and disadvantages.

⭐Side note: If you remember that the type of an Array is an object, we can create Arrays using the `new` keyword and inherit all the methods that Array constructor supports like pop, push and unshift
