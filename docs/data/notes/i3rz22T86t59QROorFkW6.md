
A transducer is a function which takes in a reducer, and returns another reducer
- A transducer takes an object or array, iterating through each value, transforming each element with a composition of transformer functions

The transduce function is really just a reduce function with an additional argument upfront:
```js
// With reduce
R.reduce(R.flip(R.append), [], autobots)

// With transduce
R.transduce(transform, R.flip(R.append), [], autobots)
```
the above reduce function iterates the elements of our `autobots` array and appends them to the accumulator

When we use `transduce()`, we are passing each item from our list into our transformation function before passing it to our reducing function. So basically, transduce is just a way for us to transform items while reducing them. We are, in fact, *transducing*

transducers are a generic and composable way to operate on a collection of values, producing a new value or new collection of new values

In this example, we can see the power of `transduce`. The first way, we need to iterate over `autobots` a total of 3 times. However, when we use `transduce` as in the second way, we only need to iterate over it once.
- The traditional way, is that we perform `map` on each element of the array, then `map` on each element again, then `filter` on each element. With transduce, we perform `map` on the first element, then `map` again on the first element, then `filter` on the first element. The resulting value is then mapped to the output array, then the second element is performed on.
```js
let autobots = ['Optimus Prime','Bumblebee','Ironhide','Sunstreaker','Ratchet']
 
// Filter for autobots that contain 'r', uppercase, then reverse
let transform = R.compose(
  R.filter(x => /r/i.test(x)),
  R.map(R.toUpper),
  R.map(R.reverse)
)
 
// BEFORE
transform(autobots)
// => [ 'EMIRP SUMITPO', 'EDIHNORI', 'REKAERTSNUS', 'TEHCTAR' ]

// AFTER
R.transduce(transform, R.flip(R.append), [], autobots)
// => [ 'EMIRP SUMITPO', 'EDIHNORI', 'REKAERTSNUS', 'TEHCTAR' ]
```

The word ‘transducer’ itself can be split into two parts that reflect this definition: 
- `transform` — to produce some value from another (ex. `reduce`/`map`).
- `reducer` — to combine the values of a data structure to produce a new one.
- this is a really cool concept, because it allows us to abstract away any implementation detais that we

`.map` and `.filter` can be implemented with `.reduce`. Therefore, we see that `.reduce` is a more general function, and `.map` and `.filter` are more specific implementations of `.reduce`. Transduce allows us to treat a reduce-like construct as a *higher order function*, meaning we can pass in the *reducing function* (which is the function found in a reducer)
- This means we could create `.map`/`.filter` with a transducer
[more info](https://www.jeremydaly.com/transducers-supercharge-functional-javascript/)