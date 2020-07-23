---
layout: post
title:  "JavaScrip Strict Mode (use strict)"
image: "/assets/social/strict.png"
date:   2020-06-15 12:00:00 -0500
---

The strict mode was introduced in ES5. It is a way of enforcing strict rules while coding in JS. Not all browsers support strict mode, so always test your code.

### Advantages:

1. Disable some silent JS errors and throw them instead.
2. Performant code in specific instances where a JS engine supports performance optimizations
3. Code improvements
   * No duplicate keys in objects
   * Variable declaration without `var` keyword
   * Error on duplicate function arguments

### Enabling strict mode

- **File-level**: add `"use strict"` on top of the file before any other statements.
- **Function level**: add the same `"use strict"` on top of the function body before any other statements.
- **Module level**: Modules introduced in ES6/ES2015 are in strict mode by default.

### Changes applied in strict mode

1. Variable declaration without `var` keyword
   ```javascript
   someVariable = 17;
   console.log(someVariable); // 17
   ```

   This is a completely valid code. Even though we did not use `let`, `var` or `const` to declare the variable, JavaScript works without an issue. This would force JS to create a global property, and that could cause side effects in a large application (variable name conflict and modifying global variables).

   Strict mode solves this by throwing an error.

2. No duplicate keys in objects
   ```javascript
   const myObject = {
        name: "Parwinder",
        age: 34,
        car: "MiniCoop",
        name: "Bhagat"
    }
   console.log(myObject.name); // Bhagat
   ```

   Again, completely valid under non-strict mode (sloppy mode) but throws an error in strict mode.

3. Throw error when trying to delete undeletable properties of an object
   ```javascript
   "use strict"
   delete Object.prototype; // throws a TypeError
   ```

4. No duplicate arguments in functions
   ```javascript
   function sumOfNumbers(a, a, b) {
       return a + a + b;
   }

   console.log(sumOfNumbers(1, 2, 3)); // 7
   ```

   The above code is valid too. It ends up with unexpected results. The value of `a` is set to 2, and the sum returns  7 instead of our expectation, 6. Strict mode throws a syntax error.

5. Assigning `NaN` to a variable has no failure feedback in sloppy mode. Strict mode throws an exception


🚨 There’s no way to cancel "use strict". Once we enter strict mode, there’s no way to turn it off.
