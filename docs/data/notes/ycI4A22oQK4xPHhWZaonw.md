
## Tagged Template Literals
```js
// These are equivalent:
fn`some string here`
fn(['some string here'])
```
- - -
The rest of the arguments will be the interpolations, in order.
```js
const aVar = 'good'

// These are equivalent:
fn`this is a ${aVar} day`
fn(['this is a ', ' day'], aVar)
```

## Identity function
```
fetchBook()
  .then((book) => formatBook(book))
  .then((postscript) => print(postscript))
```
is equivalent to (verify this)
```
fetchBook()
  .then(formatBook)
  .then(print);
```

### Passing in a named function vs Passing in a function that calls the named function
`setTimeout` will call the passed in function after X milliseconds.
- in the first case, `resolve` gets called, which resolves the promise
- in the second case, a function which calls `resolve` gets called. The act of calling this function results in `resolve` being called

Therefore, these are not the same thing, but they attain the same result

```js
const sleep = (ms) => new Promise(resolve => setTimeout(resolve, ms))
// vs
const sleep = (ms) => new Promise(resolve => setTimeout(() => resolve('foo'), ms))
```
