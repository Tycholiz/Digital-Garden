
Arrays technically aren't a type in Javascript.

Since everything in Javascript is an object, `Array` doesn't follow the same limitations as an actual [[array|general.lang.types.array]], such as being of fixed length.
- for example, the `length` property returns the size of the internal storage area for indexed items of the array. 

in JavaScript, array indexes are coerced to strings by an implicit `toString()` call.

## When to use Arrays for lists of data
If you have a list of data that you are going to be transforming somehow, or mapping over it somehow, you should try to structure that data as an array
- expl: if you had an object of keys, and each key was a list item, then to operate on that might get a little messy and imperative. Imagine if we stored taxes in an object like this:
```js
const taxes = {
    GST: 0.05,
    PST: 0.07
}
```
We can loop over this with `Object.entries` without problem, but now imagine that we want to map over each entry, and display it in the UI in React. That might look something like this:
```jsx
Object.entries(taxes).map(tax =>
    <>
        <p>Tax type: {tax[0]}</p>
        <p>Amount: {tax[1]}</p>
    </>
)
```

Alternatively, imagine we structured taxes like this:
```js
const taxes = [
    {
        jurisdiction: 'PST',
        percentage: 0.05,
    },
    {
        jurisdiction: 'GST',
        percentage: 0.05,
    },
]
```

Although this is less succinct, there is a benefit to doing it this way. This is how we would handle that React task:
```js
taxes.map(tax => {
    <>
        <p>Tax type: {tax.jurisdiction}</p>
        <p>Amount: {tax.percentage}</p>
    </>
})
```

This is yet another example of why you should go from a data-consumption perspective (in other words, consider how it will be used; think of the end-user of that code.)

