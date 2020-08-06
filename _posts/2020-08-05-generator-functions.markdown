---
layout: post
title:  "Generator Functions"
date:   2020-08-05 12:00:00 -0500
---

A function declared with `function*` that returns a `Generator` object is called a generator function. That was the bookish definition of it 😉. Generators are functions that can be exited and re-entered. The context of the function is saved through all re-entries.

### Execution
* When we call a generator function in JavaScript, it returns an iterator object.
* The iterator object has a `next()` method.
* Whenever `next()` is called, that is when the generator body is executed.
* The body is only executed **till the first** `yield` expression.
* If it is the last `yield` we get the yielded value and a `done` property to let us know that the generator has produced the last value.

#### Exception
* Instead of `yield` we can also come across `yield*`.
* `yield*` delegates to another generator function.
* `return` statement in the generator will end the generator and so will an error in the generator.

### Examples

```javascript
function* numberGenerator() { // generator function declared with function*
    let i = 0;
    while (true) {
        yield i++; // yields a generator
    }
}

const numbers = numberGenerator();

console.log(numbers.next().value); // calling next on iterator produces next yield -> 0
console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 2
console.log(numbers.next().value); // 3
```

The above example generates numbers as many time as we call `next()`. See how every execution of `next()` remembers the number to generate. The above example is relatively simple and explains the implementation of a generator function. Let us take another example that showcases `done` property.

```javascript
function* numberGenerator() {
    let i = 0;
    while (i < 3) {
        yield i++;
    }
}

const numbers = numberGenerator();

console.log(numbers.next().value); // 0
console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 2
console.log(numbers.next().done); // true
```

As you can see, when the value of `i` reaches 3, the generator sets the value of `done` property to `true`.

### Examples for Exceptions

#### Delegating to another function

```javascript
function* evenNumberGenerator() {
    yield 4;
    yield 8;
}

function* numberGenerator() {
    yield 1;
    yield* evenNumberGenerator();
    yield 33;
}

const numbers = numberGenerator();

console.log(numbers.next().value); // 1
console.log(numbers.next().value); // 4
console.log(numbers.next().value); // 8
console.log(numbers.next().value); // 33
```

When we call `numberGenerator` for the first time, the first `yield` returns 1. The next two yields are handled by `evenNumberGenerator` with values 4 and 8. Finally, we receive 33 from `numberGenerator`.

#### Generator with return

```javascript
function* numberGenerator() {
    yield 1;
    return "no more numbers"
    yield 33;
}

const numbers = numberGenerator();

console.log(numbers.next().value); // 1
console.log(numbers.next()); // { value: 'no more numbers', done: true }
console.log(numbers.next().value); // undefined
```

`numberGenerator` yields 1 on the first `next()` but marks generator as `done` on the second iteration (as soon as a `return` occurs). It does provide the value from the `return` statement.

### Passing arguments

Generator functions are capable of taking arguments as regular functions.

```javascript
function* numberGenerator() {
    console.log(0);
    console.log(1, yield);
    console.log(2, yield);
    console.log(3, yield);
}

const numbers = numberGenerator();

numbers.next(); // 0
numbers.next("uno"); // 1 "uno"
numbers.next("two"); // 2 "two"
numbers.next("tres"); // 3 "tres"
```

Keep in mind that the very first `next()` generates a 0. It is caused by the first console log statement in `numberGenerator`. As I mentioned before, when we call `next()` on a generator, the body of the function is **executed till** the first `yield`.
