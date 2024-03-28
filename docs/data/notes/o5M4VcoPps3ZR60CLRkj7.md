
every function annotated with async returns an implicit promise
- *"The async function declaration defines an asynchronous function, which returns an AsyncFunction object. An asynchronous function is a function which operates asynchronously via the event loop, using an implicit Promise to return its result."*

every time we see `await`, it means the promise must resolve before moving on. If it is rejected, then an error is thrown, and it's up to us to `catch` it.

Using async-await is fantastic when the tasks we’re trying to be accomplished are supposed to be sequential and the asynchronous function calls need to happen in series. If we need true parallelism, we need to go back to vanilla [[Promises|js.lang.promises]] .

The return value of an async function is implicitly wrapped in `Promise.resolve` - if it's not already a promise itself

`await` must appear directly inside an `async` function. Therefore, we cannot do this:
```js
const myFunc = async () => {
  const mapResult = dataFromA.map(item => {
    const dataFromB = await doApiCallHere(item.id)

    return {
      ...dataFromA,
      dataFromB,
    }
  })
}
```
Here, we are trying to use `await` within a `map` function. 
- spec: since `map` is not asynchronous, this wouldn't make sense.

If we wanted to do this, we would need to pass an `async` function to map:
```js
const mapResult = dataFromA.map(async item => {
...
```

`map` now returns an array of promises, which we can then `Promise.all` over:
```js
const myFunc = async () => {
  const promises = dataFromA.map(async item => {
    const dataFromB = await doApiCallHere(item.id)

    return {
      ...dataFromA,
      dataFromB,
    }
  })

  Promise.all(promises)
}
```

Awaiting a non-promise will have the same effect as awaiting a resolved promise that is resolved with the non-promise value. So

```js
await abc() // returns undefined
```
Is the same as

```js
await Promise.resolve(undefined)
```

The await will pause the current function (as it always does) then continue running it on the next microtask tick since no additional waiting is needed for a resolved promise and then continue on with the rest of the function.

await doesn't care if a function is async. It only looks for promises. Non async functions can also return promises though async functions always do. If it doesn't find a promise it pretends like it did by treating the value it's awaiting as a resolved promise.

## How async-await converts to Promises
The body of an async function can be thought of as being split by zero or more await expressions. 
- Top-level code, up to and including the first await expression (if there is one), is run synchronously. 
  - In this way, an async function without an await expression will run synchronously. If there is an await expression inside the function body, however, the async function will always complete asynchronously.

For example:
```js
async function foo() {
   await 1
}
```

...is equivalent to:
```js
function foo() {
   return Promise.resolve(1).then(() => undefined)
}
```

Code after each await expression can be thought of as existing in a `.then` callback.
- In this way a promise chain is progressively constructed with each reentrant step through the function. The return value forms the final link in the chain.
- the promise chain is not built-up in one go. Instead, the promise chain is constructed in stages as control is successively yielded from and returned to the async function. As a result, we must be mindful of error handling behavior when dealing with concurrent asynchronous operations. In the following example, `p2` will not be "wired into" the promise chain until control returns from `p1`.
```js
async function foo() {
   const p1 = await new Promise((resolve) => setTimeout(() => resolve('1')))
   const p2 = await new Promise((resolve) => setTimeout(() => resolve('2')))
}
```

* * *

### Top-level Await (ES2022)
Top-level await allows us to use await outside of a function marked `async`

- Essentially, our code becomes asynchronous at the module level. This can affect how your module behaves, especially if you have other code that relies on the module's exports being available immediately. Once you use top-level await, all other modules that import your asynchronous module must wait until all of the top-level awaits have been resolved.
  - Therefore, if you're not loading dependencies for your exports you might not want to be using top-level await.

In cases where your module's primary purpose is to initialize and configure something essential for the module's operation (e.g., fetching secrets), using top-level await can be a straightforward and efficient approach.

To use top-level await, you must be using ESModules, not CommonJS (ie. set the package.json field `"type": "module"`)
