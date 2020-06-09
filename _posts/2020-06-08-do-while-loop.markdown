---
layout: post
title:  "The Do While Loop"
date:   2020-06-08 12:00:00 -0500
---

## Introduction

We learned about while loops in the previous blog post. Loops allow us to repeat the same action multiple times.

The key difference between a while and a do-while loop is that the former evaluates the end condition before running the body. In contrast, the latter evaluate it at the end of body execution.

**Why does this matter?**

While while-loop is evaluating it in the beginning, if the condition is false, the body does not get executed. The do-while ensures body execution once because of the expression evaluation at the end.


A while loop looks like

```javascript
while(condition) { // If condition is false to start with, this loop will never run.
    // loop body
    // counter++
}
```

A do-while loop looks like

```javascript
do {
    statement // This statement will execute at least once before the execution of the condition below!
}
while (condition);
```

Example of a do while loop:

```javascript
let i = 0;
do {
    console.log(i); // 0, by the time condition gets evaluated this variable gets printed to the console.
} while (i != 0);
```
