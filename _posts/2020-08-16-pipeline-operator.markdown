---
layout: post
title:  "Pipeline Operator in JavaScript"
date:   2020-08-16 12:00:00 -0500
---

We all know about a ton of operators in JavaScript (AND, OR, NOT and so on). A new operator is in stage 1 for the next ES release.

🚨Before you proceed to read or use this operator, keep in mind that it might not make it to the release stage or if it does, the implementation might change.

The new operator is called pipeline operator, and it is represented as `|>` (pipe followed by greater than). The pipeline operator pipes the value of an expression into a function. It makes the chaining of function calls more readable.

### Syntax

```javascript
expression |> function
```

### Example

Let's say I have four functions I would like to chain.

```javascript
const square = input => input ** 2;
const increment = input => input + 1;
const divide = input => input / 2;
const decrement = input => input - 1;

console.log(square(increment(divide(decrement(21))))); // 121
```

It works great, and I get the expected output, but it might not be as readable. Also, the order of operation goes from right to left. With the pipeline operator it would become:

```javascript
21 |> decrement |> divide |> increment |> square;
```

It's more human-readable, and order of operation goes left to the right.

### Proposal

🚨There is absolutely no browser compatibility for this operator at the moment. There are also three proposals on how this operator should be implemented: Minimal, F# and Smart Pipelines.

Read more about [the proposal and currently under consideration implementations here](https://github.com/tc39/proposal-pipeline-operator).

Babel is gathering feedback about implementation, and you can read about it [here](https://github.com/tc39/proposal-pipeline-operator/issues/89) and [here](https://babeljs.io/blog/2018/07/19/whats-happening-with-the-pipeline-proposal).


