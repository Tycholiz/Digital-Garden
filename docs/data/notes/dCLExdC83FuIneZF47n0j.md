
Shadow DOM is a tool used to build component-based apps and websites.
- Shadow DOM comes in small pieces, and it doesn’t represent the whole DOM
- Therefore, it can be seen as a subtree or as a separate DOM for an element.

The shadow DOM is mostly used when building without a front-end framework.

### Shadow DOM vs Regular DOM
The DOM and Shadow DOM differ in how they are created and how they behave.
- DOM nodes are placed within other elements.
- Shadow DOM node (a scoped tree) is connected to the element but separated from the children elements
    - the element it's attached to is called a Shadow Host.

Everything that is added to the Shadow DOM is local, including styles.

If you have an element in the shadow dom and run `document.querySelector` to find it, it will not be returned.

If we are using [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components), then we are using the shadow DOM.

* * *

The shadow DOM API is part of the browser specification, supported by most browser versions post 2018.

Shadow DOM should not be confused with the virtual DOM, which is an implementation used in frameworks like React and Vue.

Shadow DOM allows hidden DOM trees to be attached to elements in the regular DOM tree — this shadow DOM tree starts with a shadow root, underneath which can be attached to any elements you want, in the same way as the normal DOM.