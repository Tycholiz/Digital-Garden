
When you click on a button, you are inadvertently clicking on things that inside the button, as well as things that contain the button
- ex. when you click on a strong tag, the event happens on that strong tag. But if nothing happens with that event, then it will bubble up to the surrounding element (ex. a Button). If the button wasn't listening for that specific event, then it will keep bubbling up onto the next surrounding element

Event capturing
- the process of figuring out what got clicked. Happens after the bubbling phase
- ex. when you click on a `span`, the browser starts at the top: "user clicked on the `window` > `html` > `body` > `div` > `button` > `span`"

When we add an event listener, we can specify that it triggers on the capture phase or bubble phase
- bubble phase is default, and is what we want 99% of the time.

### currentTarget vs target
`currentTarget`
- what actually got clicked, prior to any bubbling happening
- ex. we have a `span` inside a `button`. If the user clicks the `span`, then `span` is the `currentTarget` that got clicked, and the `button` is the `target`

`target`
- what you listened for a click on (what the bubbling ended up at)
