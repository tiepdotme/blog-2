---
layout: post
title:  "Finally Method on Promises"
date:   2020-08-14 12:00:00 -0500
---

Usually, if a promise is resolved, we take action in `then` block. If it errors out, we use the `catch` block. What if there is an action you would like to take regardless of the outcome? Maybe it is cleanup or displaying a message to the user that the promise has finished. The `finally()` method is used to invoke a callback function once a promise is settled no matter what the outcome. `finally` return a `Promise`.

Keep in mind that the `finally` method cannot receive any argument as there is no way to know if the promise was resolved or rejected.

### Syntax

```javascript
promise
    .then(response => {
        // handle success
    })
    .catch(error => {
        // handle error
    })
    .finally(() => {
        // take any action
    })
```

### Example

```javascript
const apiCall = (name) => {
    return new Promise((resolve, reject) => {
        if(name === "Parwinder"){
            resolve("Parwinder works here.")
        } else {
            reject(`${name} is not an employee.`)
        }
    });
}

apiCall("Parwinder")
    .then((response) => {
        console.log(response); // Parwinder works here.
    })
    .finally(() => {
        console.log("Is there anything else I can help you with?"); // Is there anything else I can help you with?
    });

apiCall("Lauren")
    .catch((err) => {
        console.log(err); // Lauren is not an employee.
    })
    .finally(() => {
        console.log("Maybe you would like to try a different employee name?"); // Maybe you would like to try a different employee name?
    })
```

In both the scenarios (resolve and reject), we can display a follow-up message using `finally`.
