
A promise is an object which can be returned synchronously from an asynchronous function. 
- This synchronous-part is reflected in the fact that a promise returns immediately, albeit in Pending state.

A promise can be in one of 3 possible states:
- *Fulfilled*: onFulfilled() will be c
- *Rejected*: onRejected() will be called (e.g., reject() was called)
- *Pending*: not yet fulfilled or rejected

If it's either fulfilled or rejected, it's said to be *settled*. Otherwise, it's *pending*.
- Once settled, a promise can *not* be resettled. Calling resolve() or reject() again will have no effect. 
  - The immutability of a settled promise is an important feature.

A promise accepts a callback function as a parameter. The callback function accepts 2 parameters, `resolve` and `reject`. 
- If the task is successfully performed, then it returns resolve. 
- Else it returns the reject.

Every promise must supply a `.then()` method with the following signature:
```ts
then: (onFulfilled?: Function, onRejected?: Function) => Promise
```

Promises make some guarantees about their use:
- Callbacks added with `then()` will never be invoked before the completion of the current run of the event loop. These callbacks will be invoked even if they were added after the success or failure of the asynchronous operation (e.g. fetch for data) that the promise represents.  Multiple callbacks may be added by calling `then()` several times. They will be invoked one after another in a synchronous way.

Promises are [[eager|general.terms.eager-lazy]], meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked. 
- If you need lazy, check out observables or tasks.

using promises effectively abstracts time out of the picture.
- abstracting away time allows us to better handle race conditions


## Components of a Promise
There are 2 sides to promises: the `executor` (the one doing the actions) and the `consumer` (the one waiting for the actions to be done so it can consume the result.
Executor ex. `(resolve, reject) =>`
Consumer ex. `.then, .catch, .finally`

```js
// Executor
var p1 = new Promise((resolve, reject) => {
  resolve('Success!');
  // or
  // reject(new Error("Error!"));
});

// Consumer
p1.then(value => {
  console.log(value); // Success!
}, reason => {
  console.error(reason); // Error!
});
```

### Executor
Executor is usually defined as part of a library, so it's often the case that this code is already written.

A executor sends the data with `resolve(data)`

Executor gets run as soon as it's defined. Therefore, its state will either be resolved or rejected

`Promise.resolve()` and `Promise.reject()` are shortcuts to manually create an already resolved or rejected promise respectively. This can be useful at times.

### Consumer
Think of `.then`, `.catch` and `.finally` as the way that consumers subscribe to the executor
A consumer receives the data (`stuff`) sent via the executor (ie. `resolve(stuff)`) with `.then(stuff)`

```js
doSomething()
  .then(result => doSomethingElse(result))
  .then(newResult => doThirdThing(newResult))
  .then(finalResult => {
    console.log(`Got the final result: ${finalResult}`);
  })
  .catch(onRejected);
```

#### `.then()`
`.then` lets us chain (ie. compose) 2+ asynchronous operations.

Signature:
- note: most often the onRejected is omitted, with responsibility delegated to a final `.catch`
```js
const then = (onFulfilled, onRejected) => Promise
```

To avoid surprises, functions passed to `then()` will never be called synchronously, even with an already-resolved promise:
- Instead of running immediately, the passed-in function is put on a microtask queue, which means it runs later (only after the function which created it exits, and when the JavaScript execution stack is empty), just before control is returned to the event loop
```js
Promise.resolve().then(() => console.log(2));
console.log(1); // 1, 2
```

`.then()` is a method available on a Promise. It doesn't even matter if the Promise is `pending`, `fulfilled`, or `rejected`.
- if we call `.then` on an already resolved/rejected promise, the `.then` block will be triggered instantly; however, the handler functions will be triggered asynchronously. To illustrate:
```js
const resolvedProm = Promise.resolve(33);

let thenProm = resolvedProm.then(value => {
    console.log("this gets called after the end of the main stack. the value received and returned is: " + value);
    return value;
});
// instantly logging the value of thenProm
console.log(thenProm);

// using setTimeout we can postpone the execution of a function to the moment the stack is empty
setTimeout(() => {
    console.log(thenProm);
});

// logs, in order:
// Promise {[[PromiseStatus]]: "pending", [[PromiseValue]]: undefined}
// "this gets called after the end of the main stack. the value received and returned is: 33"
// Promise {[[PromiseStatus]]: "resolved", [[PromiseValue]]: 33}
```

`.then` is analogous to adding another subscriber to the mailing list

.then() may be called many times on the same promise. In other words, a promise can be used to aggregate callbacks.

##### Handler functions (`onFulfilled`/`onRejected`)
if the promise is resolved (ie. when the underlying async operation is completed), `onFulfilled` is called asynchronously (to be scheduled in the current thread loop).

Signature:
```js
const onFulfilled = (fulfillmentValue) => valueOfResolveFunction
```

Handler functions have some behaviours. If it...
- returns a value, the promise returned by then gets resolved with the returned value as its value.
- doesn't return anything, the promise returned by then gets resolved with an undefined value.
- throws an error, the promise returned by then gets rejected with the thrown error as its value.
- returns an already fulfilled promise, the promise returned by then gets fulfilled with that promise's value as its value.
- returns an already rejected promise, the promise returned by then gets rejected with that promise's value as its value.
- returns another pending promise object, the resolution/rejection of the promise returned by then will be subsequent to the resolution/rejection of the promise returned by the handler. Also, the resolved value of the promise returned by then will be the same as the resolved value of the promise returned by the handler.

Think of async actions atomically, and make each step a `.then()` that returns the input for the next `.then()`
Ex. First .then() returns the JSON data as an obj, second returns a subset of that data, third acts on it

#### Rejecting Promises
`.catch(onRejected)` is short for `.then(null, onRejected)`
- If there's an exception, the browser will look down the chain for `.catch()` handlers or `onRejected`.

Whenever a promise is rejected, a [rejectionhandled event](https://developer.mozilla.org/en-US/docs/Web/API/Window/rejectionhandled_event) is sent to the global scope (probably `window`).

Just like `.then`, `.catch` returns a promise, meaning we can chain another `.then` onto `.catch` if we want to 
- this is effectively saying "do this thing, even if some previous async operation failed"

spec:when we wrap a function in `Promise(..)`, we are promisifying it
- The new function that returns this promisified function now returns a Promise that resolves to its original return value

(Inside promises) if a promise is returned (eg. return Promise.resolve('stuff')), the next .then() will execute only when that promise has resolved (with 'stuff').
If the return value is anything else besides a promise, then it will be passed immediately to the next .then()

.resolve(value)
If the value passed to it is a promise itself, this will automatically "follow" that promise chain and wait to pass back the final resolved value.
- Good to use if you are unsure if a value is a promise or not

`Promise.all` is a server at a restaurant waiting to bring everyone's food at the same time, even though one meal may be ready before the others

### Promises vs Callbacks to achieve asynchrony
With the callback method of achieving asynchrony, the idea is to pass a callback into a function and call that function after some event has occurred. 

Promises work a bit differently. Essentially, we create this object called a Promise (which if we recall, returns immediately when invoked, albeit with `[Pending]` status). To this object, we attach 2 callback functions: `resolve` and `reject`.
- we call `resolve` if the async action succeeds
- we call `reject` if the async action fails
  - most commonly the `resolve` and `reject` callbacks are asynchronous functions that return a promise. Naturally, they get called upon completion/failure of the async operation (or in the case of a promise chain, they get called when the previous promise resolves.)
  - both `resolve` and `reject` return void

* * *

## Wrapping Callbacks in Promises
In an ideal world, all asynchronous functions would already return promises. Unfortunately, some APIs still expect success and/or failure callbacks, like `setTimeout()`.

Luckily we can wrap `setTimeout` in a promise. 
- Best practice is to wrap problematic functions at the lowest possible level, and then never call them directly again:
```js
// Basically, the promise constructor takes an executor function that lets us resolve or reject a promise manually. 
// Since setTimeout() doesn't really fail, we left out reject in this case.
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));

wait(10*1000).then(() => saySomething("10 seconds")).catch(failureCallback);
```

## Promises are only ever resolved once per creation
In the following code it seems that we would connect to the database 2 times, but Promises don't work like that.
- since `databasePromise` is only defined one time, it can by definition only resolve one time.

```js
const databasePromise = connectDatabase();

const booksPromise = databasePromise
  .then(findAllBooks);

const userPromise = databasePromise
  .then(getCurrentUser);

Promise.all([
  booksPromise,
  userPromise
])
  .then((values) => {
    const books = values[0];
    const user = values[1];
    return pickTopRecommentations(books, user);
  });
```

## Sleep example
- We can use promises make a function who's purpose is to simply wait, before executing further code. We define it as such:
```ts
const sleep = (ms) => new Promise<void>(resolve => setTimeout(resolve, ms));
```
- as soon as we call `sleep(1000)`, a promise is returned to us. This means that javascript will say "ok, since we're waiting on that promise to resolve (or reject), I'm going to go do some other stuff, and once your promise resolves, I'll be back to execute the `.then()` code".
	- This is precicely why promises are said to "handle asynchronous things synchronously". It is because promises help us manage 2 different lines of execution at a time
