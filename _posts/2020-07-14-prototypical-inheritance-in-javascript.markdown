---
layout: post
title: "Prototypical Inheritance"
image: "/assets/social/inheritance.png"
date: 2020-07-14 12:00:00 -0500
---

In the very beginning on this blog we talked about how everything in JavaScript is represented as an object. When we create objects we have the inherit need of reusing its properties or methods. Most modern languages support inheritance in one way or the other. JavaScript does so by using prototypical chain or inheritance.

Every object in JavaScript has a hidden property called [[Prototype]]. It has one of two values: either `null` (marking the end of prototypical chain) or a reference to another object.

That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype. `null` has no prototype and acts as the end of the prototypical chain as I mentioned above.

### Inheriting properties from objects

```javascript
// declare an initial object animal with height and weight property
let animal = {
    height: 5,
    weight: 50
};

console.log(animal.__proto__); // null or  {}
// since animal is not inherited from anything, it doesn't have a prototypical chain

// create an object fish from animal
let fish = Object.create(animal);

console.log(fish.height); // 5, inherited from animal
console.log(fish.weight); // 50, inherited from animal
console.log(fish.__proto__); // { height: 5, weight: 50 }
// ^ chain points to animal, that is how we got fish height and weight

fish.canSwim = true; // adding a property to fish object

console.log(animal.canSwim); // undefined, it does not exist on animal. It is fish's own property
console.log(fish.canSwim); // true

let octopus = Object.create(fish); // create an object from fish

console.log(octopus.height); // 5, traversing the prototype chain octopus => fish => animal
console.log(octopus.weight); // 50, inherited all the way from animal
console.log(octopus.__proto__); // { canSwim: true }, points to fish but only shows properties that fish "owns"

octopus.legs = 8; // adding a property to octopus object

console.log(octopus.legs); // 8
console.log(animal.legs); // neither animal or fish has the legs property
console.log(fish.legs);

// hasOwnProperty method is true when an Object owns a property and did not inherit it
console.log(octopus.hasOwnProperty("legs")); // true
console.log(octopus.hasOwnProperty("height")); // false
console.log(fish.hasOwnProperty("weight")); // false
```

### `__proto__`

In the above examples we used `__proto__` to access the prototype of an object. `__proto__` is the getter and setter for [[Prototype]]. We have newer methods to do it now (`getPrototypeOf` or `setPrototypeOf`) but `__proto__` is supported by most (browsers or server side).

There are only two rules for `__proto__`:

1. At no point can a `__proto__` create a circular reference or dependency. JavaScript throws an error if we assign __proto__ in a circlcular reference.
2. As I mentioned before the value of __proto__ can be either an object or null only.

### Inheriting properties using a constructor

```javascript
let foo = function() {
    this.name = "Parwinder";
    this.age = 57;
}

let bar = new foo(); // create an object bar using function foo

console.log(bar); // foo { name: 'Parwinder', age: 57 }, due to inheritance
console.log(bar.name); // Parwinder, inherited from foo

foo.prototype.car = "Volvo"; // adding a new property "car" to original function foo

console.log(bar.car); // Volvo
// check bar if it has a property car, if not follow up the prototype chain.
// get to foo following the chain
// does foo have car on its prototype? yes. Log the value "Volvo"

console.log(bar.gender); // undefined
// check bar if it has a property gender, if not follow up the prototype chain.
// get to foo following the chain
// does foo have gender on its prototype? no.
// go up the protoypical chain.
// we have reached the end of chain with null. log undefined.
```

### Behavior of this keyword and inheritance

No matter if a method is found in an object or its prototype, `this` always refers to the object before the dot. Lets understand with an example.

```javascript
const checkVotingRights = {
  age: 24,
  legalToVote: function() {
    return this.age > 18;
  }
};

console.log(checkVotingRights.age); // 24
// When calling checkVotingRights.age in this case, "this" refers to checkVotingRights
console.log(checkVotingRights.legalToVote());

const teenagerEligible = Object.create(checkVotingRights);
// teenagerEligible is an object that inherits checkVotingRights

teenagerEligible.age = 13; // set age on the newly created object

console.log(teenagerEligible.legalToVote()); // false
// when teenagerEligible.legalToVote is called, "this" refers to teenagerEligible
```

### Using `delete` Operator with `Object.create`

Whenever a key is deleted from an object and that deleted key was inherited, logging the key will log the inherited value.

```javascript
var a = {
    a: 1
};

var b = Object.create(a);

console.log(a.a); // 1
console.log(b.a); // 1

b.a = 10;

console.log(a.a); // 1
console.log(b.a); // 10

delete b.a;

console.log(a.a); // 1
console.log(b.a); // 1, value deleted but showing value from a

delete a.a;

console.log(a.a); // undefined
console.log(b.a); // undefined
```

### Gotcha with `for..in` loop

`for..in` loop is used to iterate over properties of an object. **It loops over inherited properties as well!**

```javascript
let animal = {
    height: 5,
    weight: 50
};

let fish = Object.create(animal);

fish.canSwim = true;

for (let item in fish) {
    console.log(item); // canSwim, height, weight
}

for (let item in fish) {
    console.log(fish[item]); // true, 5, 50
}
```
