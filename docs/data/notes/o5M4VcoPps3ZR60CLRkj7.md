
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

### Top-level Await
- Top-level await allows us to use await outside of a function marked `async`
	- It acts like a big async function causing other modules who import them to wait before they start evaluating their body.
- In the latest ECMAScript, we can run `await` at the top level
