---
layout: post
title:  "Objects: Reference vs Value"
date:   2020-05-21 12:45:00 -0500
---

## Reference vs. Value

**TLDR**: In JavaScript, primitive types deal with values, whereas objects, arrays, sets, or maps work with reference!

### What is passing by value?

We talked about a bunch of types in JavaScript in my earlier posts. String, numbers or booleans work by values. What do I mean by that?

```javascript
let person = "Parwinder";
let human = person;

console.log(person); // Parwinder
console.log(human); // Parwinder

person = "Ricky";

console.log(person); // Ricky
console.log(human); // Parwinder
```

I created a variable `person` and assigned it a value. The variable `human` was equal to the variable `person` but that does not mean that `human` reflects changes in `person`. When I made `human` equal to `person` I did that by passing the value of `person` to `human`. A copy was made, and they are not related to each other. This is passing by value.

### What is passing by reference?

Objects, arrays, sets, and maps work with reference and not by value.

```javascript
let personObject = {
    firstName: "Parwinder",
    lastName: "Bhagat"
};

let humanObject = personObject;

console.log(personObject.firstName); // Parwinder
console.log(humanObject.firstName); // Parwinder

personObject.firstName = "Ricky";

console.log(personObject.firstName); // Ricky
console.log(humanObject.firstName); // Ricky
```

Did you notice the difference? The change to `firstName` of `personObject` reflects in the `firstName` of `humanObject`. That is because when I created `humanObject` and made it equal to `personObject`, it did not copy over the object. Instead, it created a reference to the `personObject`. Since both the objects point to the same reference, a change made to the reference reflects in both.

Passing by reference is not only limited to copying information. It goes beyond. One such example would be calling a function. When you call a function by passing a variable that is string, number, or boolean, it passes the value. So if we change the passed value somewhere in the function, the original value does not get impacted.

On the other hand, if I pass an Object to a function, and within the function, I change a property of the passed object, the original object gets impacted. The original object reflects the changed value now.

**In case of a primitive type**

```javascript
function changeValue(arg) {
    arg = "This is a new value";
    return arg;
}

let person = "Parwinder"
console.log(changeValue(person)); // This is a new value
console.log(person); // Parwinder
```

You can see that the variable `person` did not change when I performed an operation on variable/argument `arg`.

**In case of object**

```javascript
function changeValue(arg) {
    arg.name = "Ricky";
    return arg;
}

let person = {
    name: "Parwinder",
    age: 33
}
console.log(changeValue(person)); // { name: 'Ricky', age: 33 }
console.log(person); // { name: 'Ricky', age: 33 }
```

Whereas here, changing the name in the function did change the original object! 😱

### Then, how do I copy objects?

If you want to copy an object's values and not work with reference, you need to clone the original object. You can do this by using the **spread** (...) operator.

```javascript
let personObject = {
    firstName: "Parwinder",
    lastName: "Bhagat"
};

let humanObject = { ...personObject };

console.log(personObject.firstName); // Parwinder
console.log(humanObject.firstName); // Parwinder

personObject.firstName = "Ricky";

console.log(personObject.firstName); // Ricky
console.log(humanObject.firstName); // Parwinder
```

You can see that `humanObject` is a copy of `personObject` because when I switched the `firstName` property, it only did that change to `personObject`. The change did not propagate to `humanObject`!

### Is it that simple?

The short answer is no. What we did above using the spread operator is we made a *shallow* copy of the object. Shallow copy copies the first level properties of the object. Properties deeper than the first level are still referenced!

```javascript
let personObject = {
    firstName: "Parwinder",
    lastName: "Bhagat",
    vehicles: {
        car: "Honda Civic",
        bike: "Honda Rebel"
    }
};

let humanObject = { ...personObject };

console.log(personObject.vehicles.car); // Honda Civic
console.log(humanObject.vehicles.car); // Honda Civic

personObject.firstName = "Ricky";

console.log(personObject.firstName); // Ricky
console.log(humanObject.firstName); // Parwinder

personObject.vehicles.car = "BMW X5";

console.log(personObject.vehicles.car); // BMW X5
console.log(humanObject.vehicles.car); // BMW X5
```

In the above example, I made a shallow copy, and when I switched the name in one object, it did not change in the other (as expected). But when I change the car which is not on the first level of the object, it gets changed in the other object. Remember, shallow copy only copies the first level, deeper levels are still by reference.
