
When async/await was first introduced, attempting to use an `await` outside of an async function resulted in a SyntaxError. Many developers utilized immediately-invoked async function expressions as a way to get access to the feature.

```js
await Promise.resolve(console.log('🎉'))
// → SyntaxError: await is only valid in async function

;(async function() {
  await Promise.resolve(console.log('🎉'))
  // → 🎉
}())
```
- note: this IIFE here could be considered to be analogous to the `main` function of a C program (ie. it is the entrypoint— it is what gets called when you run `node filename.js`). Otherwise, we would be declaring the function, then calling it

Without top-level await, JavaScript developers often used async immediately-invoked function expressions just to get access to await. Unfortunately, this pattern results in less determinism of graph execution and static analyzability of applications. For these reasons, the lack of top-level await was viewed as a higher risk than the hazards introduced with the feature.
