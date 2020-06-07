---
layout: post
title:  "Event Capturing"
date:   2020-06-06 12:00:00 -0500
---

## Introduction

Event capturing is the opposite of Event bubbling. In the case of event bubbling, events bubble up from the element parent and then its ancestors.

In the case of event capturing, it starts from the top, going down the DOM structure until it reaches the target element. The target element is common to bubbling and capturing. Capturing ends at target, and bubbling starts at target.

Event capturing is rarely used. To enable event capturing, we can pass the third parameter to `addEventListener`.

Example:

```javascript
const myButton = document.querySelector(".btn-primary");

myButton.addEventListener("click", function() {
    console.log("The button was clicked");
}, { capture : true });
```

The third parameter set to true enables the capturing phase. Now when an event happens, it starts at the top, trickles down to the target element, and eventually bubbles up.

The third parameter does not need to be an object. It can be a boolean `true`.

```javascript
myButton.addEventListener("click", function() {
    console.log("The button was clicked");
}, true);
```

To summarize, DOM Events have three phases:

1. Capturing
2. Target
3. Bubbling

We can determine the phase we are in by using `event.eventPhase` or the event handler.

🚨If `addEventListener` used `true` as the third parameter for event capturing, we mention the same phase in `removeEventListener` to remove the handler correctly.
