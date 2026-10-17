
`pipe(..)` is identical to `compose(..)` except it processes through the list of functions in left-to-right order:
`var pipe = reverseargs( compose )`

the function returned by `pipe` takes the same number of arguments as the first function `.pipe()` is given

when we call `operate(3, 4)`, pipe passes the 3 and 4 to the `multiply` function, resulting in 12. it passes that 12 to addone, which returns 13. it then passes that 13 to square, which returns 169, and that becomes the final result of operate.
```js
const operate = pipe(
  multiply,
  addOne,
  square
)
```

ex. improvement with `R.pipe`
```js
// before
const titlesForYear = (books, year) => {
  const selected = filter(publishedInYear(year), books)
 
  return map(book => book.title, selected)
}

// after
const titlesForYear = year =>
  pipe(
    filter(publishedInYear(year)),
    map(book => book.title)
  )
```
