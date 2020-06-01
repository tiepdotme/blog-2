---
layout: post
title:  "JavaScript Console Methods"
date:   2020-05-31 15:00:00 -0500
---

## What is the console object in JavaScript?

The `console` object provides access to the debugging console. It is a global object, and it can be accessed from anywhere. Troubleshooting your code is standard with any programming language. Console methods make it easier to log statements, variables, functions, or errors.

```javascript
console.log("Running the program..."); // Running the program...
```

`log` is the most commonly used method. It displays the message you pass to it. `log` is used for general purpose logging.

### Assert

`console.assert()` logs a message and stack trace to the console when the first argument is false.

```javascript
const error = "Number is not divisible by 2";
console.assert(5 % 2 === 0, { errorMsg: error }); // Assertion failed: { errorMsg: "Number is not divisible by 2" }
console.assert(4 % 2 === 0, { errorMsg: error }); // No output for this statement as assertion is true
```

### Clear

Clears the console if the environment allows it.

### Dir

`console.dir()` displays an interactive list of properties of the specified JavaScript object. For example, if I visit `www.google.com` and execute the following in browser console.

```javascript
console.dir(document.location);
```

`dir` logs all properties of the location object provided by browser document.

![Console Dir](/blog/assets/console-dir.png "Listing Document Location Object")

### Error

`console.error()` is similar to the `log` method. It is used to output error messages.

```javascript
console.error("Process exited with code 1"); // Process exited with code 1
```

### Log

```javascript
console.log("Running the program..."); // Running the program...
```

`log` is the most commonly used method. It displays the message you pass to it. `log` is used for general purpose logging.

`log` and `dir` are similar in many aspects but differ in how they output. Given an object, `dir` outputs an interactive list of properties for easy navigation whereas `log` outputs the string representation.

### Table

`console.table()` is the most underrated console method. It displays data in tabular form, making it easier to read. The data should be displayable in a table (array or object).

In the case of an array, the table consists of an index column and a value column. For an object, the table has a column for keys and another for values.

`table` also takes an optional argument of the column label that is included in the output.

![Console Table Example](/blog/assets/console-table.png "Table Representation of Array")

![Console Table Example](/blog/assets/console-table-objects.png "Array of Objects Table")

### Time

`console.time()` creates a timer with a given name/label. You can have up to 10,000 timers running on a page. These are used to time how long an operation takes.

### timeEnd

Allows you to end the timer created using `console.time`

### Trace

Use to output a stack trace to the console.

### Warn

`console.warn()` outputs a warning message to the console. If you use it in Firefox or Chrome, you see a yellow exclamation informing you about the warning.
