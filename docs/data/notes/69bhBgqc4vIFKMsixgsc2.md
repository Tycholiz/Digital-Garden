
There are two iteration protocols: *iterable protocol* and *iterator protocol*.

### Iterator protocol
An object is an iterator when it implements an interface that answers two questions:
- Is there any element left?
- If there is, what is the element?

Technically speaking, an object is qualified as an iterator when it has a `next()` method that returns an object with two properties:
- `done`: a boolean value indicating whether or not  there are any more elements that could be iterated upon.
- `value`: the current element.

If we call `next()` after the last value has returned, we are returned:
```js
{done: true: value: undefined}
```

Array, Maps, and Sets have built-in iterators (ie. the protocol is already implemented)
- If you have a custom type and want to make it iterable so that you can use the `for...of` loop construct, you must implement the iteration protocols by hand.

### Iterable protocol
An object is iterable when it contains a method called `[Symbol.iterator]` that takes no argument and returns an object which conforms to the iterator protocol (ie. has `next()`, and an object `{ value: x, done: false }`).
- The `[Symbol.iterator]` is one of the built-in well-known symbols in ES6.

### Iterable
An iterable is an object that defines its iteration behavior. 

Custom iterators and iterables are useful, but are complicated to build, since you need to take care of the internal state. A generator is a special function that allows you to write an algorithm that maintains its own state. They are factories for iterators. A generator function is a function marked with the * and has at least one yield-statement in it.

Iterable objects are a generalization of arrays. That's a concept that allows us to make any object useable in a for..of the loop.

The iterable is an interface that specifies that an object can be accessible if it implements a method who is key is `[symbol.iterator]`.

# E Resources
[Roll your own iterable object](https://www.javascripttutorial.net/es6/javascript-iterator/)
