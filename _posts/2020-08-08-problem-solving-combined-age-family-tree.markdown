---
layout: post
title:  "Problem Solving: Find Combined Age From a Family Tree"
date:   2020-08-08 12:00:00 -0500
---

### Problem Statement

Give a family tree, find the sum of ages.

* The family tree is represented as an object.
* Children in the family are represented as an array in the object.
* The tree could go `n` depth deep.
* Some family members might not have children.

```javascript
const family = {
    name: "Bob",
    age: 50,
    children: [
        {
            name: "Emily",
            age: 30,
            children: [
                {
                    name: "Mary",
                    age: 4
                },
                {
                    name: "Robert",
                    age: 2
                }
            ]
        },
        {
            name: "Parwinder",
            age: 28
        },
        {
            name: "Eugene",
            age: 25
        }
    ]
}
```

🚨 If you would like to solve the problem, give it a try now. Do not read further. The next part provides you with a solution.

### Solution

```javascript
let combinedAge = family.age; // start with the topmost parent's age, we will accumulate into this

const getCombinedAge = (x) => {
    if (x.children) { // if children exist
        x.children.forEach(element => { // loop over all children
            combinedAge += element.age; // add their ages
            if (element.children) { // if further children are present
                getCombinedAge(element); // run recursively for nested children
            }
        });
        return combinedAge;
    } else {
        return 0;
    }
}

console.log(getCombinedAge(family)); // 139
```

I am sure a better solution exists. If you do know, please let me know! If there is a use case in which my solution will fail, please update me about it. I am doing this at midnight, so the brain cells are falling asleep!

Happy coding 👋🏼
