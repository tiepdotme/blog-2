---
layout: post
title:  "Problem Solving: Reverse words in a sentence"
date:   2020-08-07 12:00:00 -0500
---

### Problem Statement

Write a function that takes a string of one or more words as an argument and returns the same string, but with all five or more letter words reversed in place.

* Strings passed in will consist of only letters and spaces.
* Spaces will be included only when more than one word is present.
* You can expect a valid string to be provided for each test case.

#### Examples

```javascript
reverse("Reverse") ➞ "esreveR"
reverse("This is a typical sentence.") ➞ "This is a lacipyt .ecnetnes"
reverse("The dog is big.") ➞ "The dog is big."
```

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a solution.

### Solution

```javascript
const reverse = (input) => {
    let output = "";
    const wordArray = input.split(" "); // gives us an array of words

    for (let i = 0; i < wordArray.length; i++) { // loop over all words
        if (output) {
            output += ' '; // add a space after every word
        }

        if (wordArray[i].length >= 5) { // if the current word is greater than length 5
            output += wordArray[i].split("").reverse().join(""); // add it to output reversed
        } else {
            output += wordArray[i]; // add it to output as is
        }

    }
    return output;
}

console.log(reverse("Reverse")); // esreveR
console.log(reverse("This is a typical sentence.")); // This is a lacipyt ecnetnes.
console.log(reverse("The dog is big.")); // The dog is big.
```

Above solution is the long way to achieve the result. Even though it is a lot of code, it is easy to understand and uses traditional for-loop and concatenation. Believe it or not, but we can provide a one-line solution 😲!

```javascript
const reverse = (input) => input.replace(/\w{5,}/g, word => word.split('').reverse().join(''));
```

It might be less readable, but it is for sure concise. We can also tweak the above solution slightly and use the spread operator.

```javascript
const reverse = (input) => {
    return input.replace(/\w{5,}/g, (substring) => {
        return [...substring].reverse().join('');
    });
}
```
