
every function annotated with async returns an implicit promise
- *"The async function declaration defines an asynchronous function, which returns an AsyncFunction object. An asynchronous function is a function which operates asynchronously via the event loop, using an implicit Promise to return its result."*

every time we see `await`, it means the promise must resolve before moving on. If it is rejected, then an error is thrown, and it's up to use to `catch` it.

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
Here, we are trying to use `await` within a `map` function. spec: since `map` is not asynchronous, this wouldn't make sense.

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


### Top-level Await
- Top-level await allows us to use await outside of a function marked `async`
	- It acts like a big async function causing other modules who import them to wait before they start evaluating their body.
- In the latest ECMAScript, we can run `await` at the top level
