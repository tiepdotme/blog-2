---
layout: post
title: "Destructuring: Arrays"
date: 2020-06-27 12:00:00 -0500
---

Destructuring or destructuring assignment is a syntax that allows us to unpack arrays or objects into variables. This blog post will go over array destructuring.

To illustrate, let's look at an example. We will create a function that takes an array of numbers and displays the numbers.

```javascript
const myNumbers = (arrOfNumbers) => {
    const a = arrOfNumbers[0];
    const b = arrOfNumbers[1];
    const c = arrOfNumbers[2];
    const d = arrOfNumbers[3];
    const e = arrOfNumbers[4];
    const f = arrOfNumbers[5];
    const g = arrOfNumbers[6];

    console.log(a, b, c, d, e, f, g)
}

myNumbers([7, 2, 19, 4000, 12, 45, -17]); // 7 2 19 4000 12 45 -17
```

Above is fine, but we have to assign variables to each array index, and it is a ton of repetitive code. You could also loop over the array.

```javascript
const myNumbers = (arrOfNumbers) => {
    arrOfNumbers.forEach((value) => {
        console.log(value);
    })
}

myNumbers([7, 2, 19, 4000, 12, 45, -17]); // 7, 2, 19, 4000, 12, 45, -17
```

Looping works but now we have added logic.

Destructuring simplifies this.

```javascript
const myNumbers = (arrOfNumbers) => {
    const [a, b, c, d, e, f, g] = arrOfNumbers;
    console.log(a, b, c, d, e, f, g); // 7 2 19 4000 12 45 -17
}

myNumbers([7, 2, 19, 4000, 12, 45, -17]);
```

As simple as that! Destructuring maps the left side of the expression to the right and assigns those values.

### Using default values

Not every time the left or right-hand side of the equation will be the same in length/size. We can assign default values in such cases.

```javascript
let a, b;

[a=19, b=-17] = [1];
console.log(a); // 1
console.log(b); // -17
```

It assigned 1 to `a`, but that was the end of the array. `b` got the default value of -17. When there are extras on the right-hand side, they are ignored.

```javascript
let a, b;

[a = 19, b = -17] = [1, 2, 3,];
console.log(a); // 1
console.log(b); // 2
```

### Swapping variables without temporary variables

```javascript
let a = 5;
let b = 15;

[a, b] = [b, a];
console.log(a); // 15
console.log(b); // 5
```

### Using it with function returns

```javascript
function foo() {
  return [1, 2];
}

let a, b;
[a, b] = foo();
console.log(a); // 1
console.log(b); // 2
```

### Ignoring specific values

There are times when the values you are interested in are not in order in an array. We can skip the intermediate values.

```javascript
function foo() {
    return [1, 2, 3, 4];
}

let a, b;
[a, , , b] = foo();
console.log(a); // 1
console.log(b); // 4
```

### Using with strings

`split` string method comes in handy when we want to combine destructuring and strings.

```javascript
const [firstName, lastName] = "Parwinder Bhagat".split(' ');
console.log(firstName); // Parwinder
console.log(lastName); // Bhagat
```

### Assigning to objects

```javascript
let user = {};
[user.firstName, user.lastName] = ["Parwinder", "Bhagat"];

console.log(user); // { firstName: 'Parwinder', lastName: 'Bhagat' }
```

### Destructuring and rest (...) operator

If we are interested in the first few values, but also want to gather all the following, we can use the rest (...) operator to save the **rest** of them!

```javascript
let [name1, name2, ...remaining] = ["Parwinder", "Lauren", "George", "Eliu", "Gaurav"];

console.log(name1); // Parwinder
console.log(name2); // Lauren

console.log(remaining.length); // 3
console.log(remaining[0]); // George
console.log(remaining); // [ 'George', 'Eliu', 'Gaurav' ]
```
