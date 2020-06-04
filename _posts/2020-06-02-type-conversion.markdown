---
layout: post
title:  "Type Conversion"
date:   2020-06-02 15:00:00 -0500
---

## To Binary

JavaScript has two boolean values: `true` and `false`. But, it also treats certain values as `truthy` or `falsy`. All values are `truthy` except `0`, `null`, `undefined`, `""`, `false`, and `NaN`.

We can switch values between true and false using the negation operator `!`. This conversion also converts the type to `boolean`.

```javascript
const a = null;
const b = undefined;
const c = "";
const d = 0;

console.log(typeof a); // object
console.log(typeof b); // undefined
console.log(typeof c); // string
console.log(typeof d); // number

const w = !a;
const x = !b;
const y = !c;
const z = !d;

console.log(typeof w); // boolean
console.log(typeof x); // boolean
console.log(typeof y); // boolean
console.log(typeof z); // boolean
```

This did change the type to boolean, but it also switched the value. If you need conversion to boolean but stay on the same `truthy` or `falsy` side, use `!!` 🤯

```javascript
const a = null;
const b = undefined;
const c = "";
const d = 0;

console.log(typeof a); // object
console.log(typeof b); // undefined
console.log(typeof c); // string
console.log(typeof d); // number

const w = !!a;
const x = !!b;
const y = !!c;
const z = !!d;

console.log(typeof w); // boolean
console.log(typeof x); // boolean
console.log(typeof y); // boolean
console.log(typeof z); // boolean

// Let's check if they are all false though and haven't switched to true!

console.log(w); // false
console.log(x); // false
console.log(y); // false
console.log(z); // false
```

## To String

Use `toString()` method.

```javascript
const num = 7;
console.log(typeof num); // number
const numString = num.toString();
console.log(typeof numString); // string
```

Or use the shortcut by appending to `""` 🤯

```javascript
const num = 7;
console.log(typeof num); // number
const numString = num + "";
console.log(typeof numString); // string
```

## To Number

`parseInt()` function parses a string and returns an integer. You pass the string as the first parameter and the second parameter is radix. It specifies which numeral system to use: hexadecimal (16), octal (8), or decimal (10).

```javascript
console.log(parseInt("0xF", 16)); // 15
console.log(parseInt("321", 10)); // 321
```

Or use the shortcut by adding `+` operator in front of the string! 🤯

```javascript
console.log(+"0xF"); // 15
console.log(+"321"); // 321
```

There are situations where the `+` operator might be used for concatenation. In that case, use the bitwise NOT operator ~ twice.

```javascript
console.log(~~"0xF"); // 15
console.log(~~"321"); // 321
```
