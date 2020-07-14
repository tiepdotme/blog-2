---
layout: post
title: "10 Methods for String Reversal in JavaScript"
date: 2020-07-13 12:00:00 -0500
---

String reversal is common in day to day activities and as an important interview question. We will go over ten methods to do string reversal today.

### Using in-built methods

Convert string into an array of characters, reverse the array and the join all characters in the array.

```javascript
const stringReverse = str => str.split("").reverse().join("");

const string1 = "contrary to popular belief, Lorem Ipsum is not simply random text";
const string2 = "there are many variations of passages of Lorem Ipsum available";

console.log(stringReverse(string1));
// txet modnar ylpmis ton si muspI meroL ,feileb ralupop ot yrartnoc
console.log(stringReverse(string2));
// elbaliava muspI meroL fo segassap fo snoitairav ynam era ereht
```

### Using the traditional `for` loop

Loop over the string from the last character to the first. Yes, a string is iterable in JavaScript. We get the last character using the string `length` property.

```javascript
const stringReverse = str => {
    let output = "";
    for (let i = str.length - 1; i >= 0; i --) {
        output += str[i];
    }
    return output;
}
```

### Using `for..of` syntax

```javascript
const stringReverse = str => {
    let output = "";
    for (let char of str) {
        output = char + output;
        console.log(output)
    }
    return output;
}
```

You would think that the `for..of` loop goes from left to right when reading text. You are right. Then how does this work? Notice that we did not concatenate the string using `+=` in this example as I did in the last one. We are appending each character **before** the output. 🤯

🚨 `for..of` is not supported in Internet Explorer. If you are supporting IE, use a fallback or avoid the above example.

#### Using the while loop

```javascript
const stringReverse = str => {
    let output = "";
    let i = str.length;
    while (i--) { // while loops run until the value is "falsy"
        output += str[i];
    }
    return output;
}
```

### Using the `for..in` syntax

```javascript
const stringReverse = str => {
    let output = "";
    for (let char in str) {
        output = str[char] + output;
    }
    return output;
}
```

🚨 `for..in` provides you with the letter index. The implementation performs poorly, and I will personally refrain from it.

### ES6 `map` method

```javascript
const stringReverse = str => {
    let output = "";
    str.split("").map(char => {
        output = char + output;
    });
    return output;
}
```

`map` works on Arrays and we know that we can convert strings into Arrays from our very first example (using `split`).

### ES6 `forEach`

```javascript
const stringReverse = str => {
    let output = "";
    str.split('').forEach(char => {
        output = char + output;
    });
    return output;
}
```

### Recursion

```javascript
const stringReverse = str => {
    if(!str){
        return str;
    } else {
        return stringReverse(str.substr(1)) + str[0];
    }
}
```

We keep calling the same method `stringReverse` with the subset of the original string until we run out of string to process. We can shorten the above function (albeit lose a bit of readability).

```javascript
const stringReverse = str => str ? stringReverse(str.substr(1)) + str[0] : str;
```

### Using the ES6 spread (...) operator

```javascript
const stringReverse = str => [...str].reverse().join('');
```

### ES6 reduce method

Yes, using the `reduce` method.
But why? 😕
Why not?

```javascript
const stringReverse = str => {
    return str.split("").reduce(function(output, char){
       return char + output;
  }, "");
}
```

🚨 Keep in mind that I have used the variable `output` in a lot of examples. A lot of times it is not necessary if you are replacing string in place, you are not worried about space complexity or mutating the string will not have any side effects.
