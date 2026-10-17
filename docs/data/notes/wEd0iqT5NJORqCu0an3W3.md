

The following function uses closure to remember the pair
```ts
function makePair<F, S>() {
  let pair: { first: F; second: S }
  function getPair() {
    return pair
  }
  function setPair(x: F, y: S) {
    pair = {
      first: x,
      second: y
    }
  }
  return { getPair, setPair }
}

const { getPair, setPair } = makePair<string, number>()
setPair(5, 3)
// error: Argument of type 'number' is not assignable to parameter of type 'string'.

setPair("age", 3)
// all good!
```
