---
layout: post
title:  "Event Bubbling in JavaScript"
image: "/assets/social/event-bubbling.png"
date:   2020-06-05 12:00:00 -0500
---

## Introduction

We talked about events and event handling in the previous post. We can attach handlers to events. These handlers get invoked when the event happens.

Event bubbling in JavaScript means that when an event happens on an element, its handler is invoked, followed by its parents and then the ancestors.

Example:

```html
<div onclick="alert('Click Event Happened')">
    <p>If you have click this paragraph in the browser, the onclick handler of the div will get invoked.</p>
</div>
```

The click on the paragraph in the example above will invoke the `onclick` alert from the div. The event has **bubbled** up from the paragraph to the div element.

This would happen even if we had n number of nested elements with event handlers on them.

Example:

```html
<span onclick="alert('Span Clicked')">
    <div onclick="alert('Div Clicked')">
        <p onclick="alert('Paragraph Clicked')">Click Me.</p>
    </div>
</span>
```

If we click the paragraph in the above example, it will cause three alerts in the browser window!

## Finding the original event element

When an event bubbles through multiple layers, it is hard to keep track of what caused the chain of events. JavaScript makes it easy.

In the above example, if we clicked the paragraph, then the paragraph would be referred to as the `target` or `event.target`. No matter how many layers an event bubbles up, the `event.target` never changes. This tells us the original item where the event happened.

Now let us say that the event has bubbled up, and we want to know the current item in the event bubble chain. That is easy, as well. The keyword `this` or `event.currentTarget` will provide you with the current processing element.

## How to prevent events from bubbling up?

Events that bubble up will go all the way to `<html>` element. Some do go to `document,` and some of them make it to the `window` object.

If we do not want any handlers from the parent elements to get invoked, use `event.stopPropagation()`.

Example:

```html
<div onclick="alert('This will not alert')">
  <button onclick="event.stopPropagation()">Click me</button>
</div>
```

If we click the button with `stopPropagation()`, the div event handler or alert will not get triggered.

## How to prevent multiple events on the same element?

There are times when one element could have multiple event handlers on it for one event. We might want to stop bubbling up but also want any other handlers of the element from getting invoked.

`event.stopImmediatePropagation()` makes sure there is no event bubbling up or any other handle execution on the element.

## Why stopping event bubbling could be wrong?

If we have a document level click event listener (for tracking, analytics, or invoking popups), these will not get triggered when the end-user clicks on an element with `stopPropagation()`. So be careful when we prevent event bubbling. This is just one example, but there could be multiple side effects.

## Exceptions to event bubbling

Not all the events bubble. Any event that has to do with one specific element does not bubble. Few such events are:

* load
* unload
* focus
* blur
