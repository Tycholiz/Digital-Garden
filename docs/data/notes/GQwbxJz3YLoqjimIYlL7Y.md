
`console.X()` methods return undefined

### `console.table`
If we have an array of identically formatted objects, we can print out a nice table to the console, instead of getting a dump of a JSON-like structure, as we would have with a simple console.log

### `console.group`

* * *

### Console.log an arrow function
One of the pain points of implicitly returning arrow functions is that you can't easily stick a `console.log` in, without converting it to an explicit return. To get around this, we can have the function return `console.log(myVal) || <rest of the code>`. The reason this works is because console methods return `undefined`, which causes `<rest of the code>` to continue to be executed.

```js
// Before: 
const myFunc = () => doTheThing()

// After:
const myFunc = () => console.log(myVal) || doTheThing()
```
