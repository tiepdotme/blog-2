---
layout: post
title:  "Events & Event Handling"
image: "/assets/social/events.png"
date:   2020-06-04 12:00:00 -0500
---

## Events

Any action (be it system or user-generated) that you can react to is called an event. How we react/respond to it is called event handling.

For example, when a user clicks on a button, we might want to show them a popup with a message. In this case, the event is `click`, and the handler displays the popup.

There are a ton of events that happen on a webpage:

1. User hovering over an element
2. A form is submitted
3. A video finished playing
4. User scrolls past an image
5. Resizing of the browser window
6. Key presses
7. Document finished loading

All of these are **events** that can be responded to. Check out this massive list of [available events](https://developer.mozilla.org/en-US/docs/Web/Events).

## Event Handler

I briefly mentioned it above, but an event handler is how we respond to an event. It is a block of code that executes when an event fires.

We do use `event listeners` and `event handlers` interchangeably, and you are free to do so.

However, there is a small technical distinction. `listeners` listen to when an event happens, and the `handler` is the code that gets executed.

Whenever we attach an event handler to an event, we refer to it as registering an event handler.

## Examples

Let us assume that our webpage has a button.

```html
<button class="btn-primary">Click Me!</button>
```

We can attach an event listener to it and display a message when someone does a `click` event.

```javascript
const myButton = document.querySelector(".btn-primary");

myButton.addEventListener("click", function() {
    console.log("The button was clicked");
});
```

What is happening here:
1. We grab the button from our browser DOM (document object model) using the `querySelector` method. `querySelector` provides us with the **Node** of the button.
2. We add an event listener with `addEventListener`.
3. `addEventListener` takes two arguments.
4. The first argument for `addEventListener` is the event type. In this case, we have specified `click`.
5. The second argument is a callback function that will get executed once a click happens.

The browser would know that whenever the user clicks on the button, there is a `click` event registered to this button with class `btn-primary`. It will then execute the handler associated with the event. The event handler will print out

> The button was clicked

The callback method provided to the event listener is an anonymous function. The function does not have a name, and it cannot be referenced from anywhere. We do not always have to use an anonymous function. We can create a named function and pass that. A named function is preferred for re-usability and giving us the ability to remove the event listener later.

### Using named function

The event handler could be a named function.

```javascript
const myButton = document.querySelector(".btn-primary");

const handleClick = function() {
    console.log("The button was clicked");
};

myButton.addEventListener("click", handleClick);
```

Not only does this look cleaner, but it also has two advantages.

1. **Reusability**:
   Imagine you have more than one button that prints the same console statement. A named function will be used multiple times without repeating code.
   ```javascript
   const myButton = document.querySelector(".btn-primary");
   const secondButton = document.querySelector(".btn-second");
   const handleClick = function() {
       console.log("The button was clicked");
   };
   myButton.addEventListener("click", handleClick);
   secondButton.addEventListener("click", handleClick);
   ```
2. **Removing event listener**:
   Removing event listeners is done using `removeEventListener`. We need to pass two key arguments to remove an event listener. The first being the event, and the second is the event handler method. We cannot specify the second parameter if the callback was an anonymous function. In the case of a named function, we would do:
   ```javascript
   myButton.removeEventListener("click", handleClick);
   ```
