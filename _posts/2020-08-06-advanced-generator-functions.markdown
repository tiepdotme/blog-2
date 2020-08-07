---
layout: post
title:  "Advanced Generator Functions"
date:   2020-08-06 12:00:00 -0500
---

We went over generator functions in the last blog post. Now is the time to go over some edge cases and advanced topics for generators.

### Generators can be object properties

```javascript
const objectWithGenerator = {
    *numberGenerator() {
        yield "1";
        yield "2";
    }
}

const numbers = objectWithGenerator.numberGenerator();

console.log(numbers.next()); // { value: '1', done: false }
console.log(numbers.next()); // { value: '2', done: false }
console.log(numbers.next()); // { value: undefined, done: true }
```

### Generators can be class methods

```javascript
class classWithGenerator {
    *numberGenerator() {
        yield "1";
        yield "2";
    }
}

const generatorObject = new classWithGenerator;
const numbers = generatorObject.numberGenerator();

console.log(numbers.next()); // { value: '1', done: false }
console.log(numbers.next()); // { value: '2', done: false }
console.log(numbers.next()); // { value: undefined, done: true }
```

### Generators can be defined as expressions!

```javascript
const objectWithGenerator = function* () {
    yield "1";
    yield "2";
}

const numbers = objectWithGenerator();

console.log(numbers.next()); // { value: '1', done: false }
console.log(numbers.next()); // { value: '2', done: false }
console.log(numbers.next()); // { value: undefined, done: true }
```

### Generators are **not** constructors

```javascript
const objectWithGenerator = {
    *numberGenerator() {
        yield "1";
        yield "2";
    }
}

const numbers = new objectWithGenerator(); // objectWithGenerator is not a constructor 
```
