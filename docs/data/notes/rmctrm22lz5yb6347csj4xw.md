
### NOT operator (`~`)
the tilde `~` Bitwise NOT operator is commonly used right before an indexOf() to do a boolean check (truthy/falsy) on a string.

```js
// prior to ES7
~foo.indexOf("w")

// As of ES7
foo.includes("w")
```