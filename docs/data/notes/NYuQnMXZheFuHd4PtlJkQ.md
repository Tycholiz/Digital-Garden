
### Async iterator
Asynchronous iteration allow us to iterate over data that comes asynchronously, on-demand.
- ex. download something chunk-by-chunk over a network.
- in other words, it allows you to access asynchronous data sequentially.

Async iterators can be used when we don’t know the values and the end state we iterate over. Instead, we get promises.

`Symbol.asyncIterator` is a built-in symbol that enables an object to be async iterable.
- This enables us to use a `for-await-of` loop with async iterators, which allows us to loop over the async iterator.
    - Imagine having an array of promises, and each promise represents some data that will arrive in the next chunk. We can loop over each promise, and do something with that data when it arrives. Only once the current promise has resolved will it move onto the next.
- `Symbol.asyncIterator` is a method that returns the default AsyncIterator for an object. If the `asyncIterator` property is on an object, it is an Async Iterable, and therefore we can use `for-await-of`.

An async iterator is like an iterator except that its `next()` method returns a promise that resolves to the `{value, done}` object.


There are currently no built-in JavaScript objects that have the `[Symbol.asyncIterator]` key set by default.
- Streams do, but they are from Node.js. In fact, async iterators are very useful when dealing with streams

A great use case for async iteration is when we need to fetch data from a remote paginated dataset.
- https://www.nodejsdesignpatterns.com/blog/javascript-async-iterators/

Since the ability of an iterator to be async is determined by the presence of a `[Symbol.asyncIterator]()` property on an object, we can define our own:
```js
const myAsyncIterable = {
    async* [Symbol.asyncIterator]() {
        yield "hello";
        yield "async";
        yield "iteration!";
    }
};

(async () => {
    for await (const x of myAsyncIterable) {
        console.log(x);
        // expected output:
        //    "hello"
        //    "async"
        //    "iteration!"
    }
})();
```

Another example:
```js
const asyncIterable = [1, 2, 3];
asyncIterable[Symbol.asyncIterator] = async function*() {
    for (let i = 0; i < asyncIterable.length; i++) {
        yield { value: asyncIterable[i], done: false }
    }
    yield { done: true };
};

(async function() { 
    // The for-await-of loop will wait for every promise it receives to resolve before moving on to the next one
    for await (const part of asyncIterable) {
        console.log(part);
    }
})();
```

#### Consuming paginated APIs
https://blog.risingstack.com/async-iterators-in-node-js/
