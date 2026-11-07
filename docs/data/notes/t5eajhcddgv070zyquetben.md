
Asynchronously observe changes in the intersection of a target element with an ancestor element (often, this ancestor element is the viewport itself)
- ex. we can detect when a certain element has entered the viewport

### Why use this?
- Lazy-loading of images or other content as a page is scrolled.
- Implementing "infinite scrolling" websites, where more and more content is loaded and rendered as you scroll, so that the user doesn't have to flip through pages.
- Reporting of visibility of advertisements in order to calculate ad revenues.
- Deciding whether or not to perform tasks or animation processes based on whether or not the user will see the result.
- Tracking user engagement as they scroll through a page.

### Example
Imagine you have a web page with a long list of items, and you want to know when an item becomes visible in the user's viewport as they scroll down the page. In this case:

By using the Intersection Observer API, you can set up observers on each item element to detect when it becomes visible within (ie. intersects) the viewport. When an item enters or leaves the viewport, you can asynchronously trigger actions, such as lazy loading images or animating the item's appearance.