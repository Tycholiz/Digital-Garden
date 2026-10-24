
### Run loop indefinitely, and stop when a certain condition is met
```js
for (let turn = 0;; turn++) {
  if (state.parcels.length == 0) {
    console.log(`Done in ${turn} turns`);
    break;
  }
  // do your stuff here
}
```

## `label`
A `label` allows us to identify a statement, and then later refer to it using a `break` or `continue` statement

Any break or continue that references label must be contained within the statement that's labeled by label. Think about label as a variable that's only available in the scope of statement.

Imagine we have 2 nested loops and the inner loop has some condition whereby it should return execution to the outer loop. In this case, what we would do is label the outer loop and create the condition from within the inner loop.

```js
loop1: for (let i = 0; i < 3; i++) {
  loop2: for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      continue loop1;
    }
    console.log(`i = ${i}, j = ${j}`);
  }
}
```

We can also use `break` with labels. In this case, if `break loop1` is encountered from within the inner loop, then the execution of the outer loop is terminated.

## `for..of` vs `for..in`
`for..of` should be used to loop over iterable objects, like [[Array|js.lang.array]], [[Map|js.lang.type.map]], [[Set|js.lang.type.set]], String, [[Generator|js.lang.functions.generator]]
- an object is considered iterable if it adheres to the [[iterator protocols|js.lang.iterator]]
`for..in` should be used with objects, and is used to iterate over the properties of an object

### For..of
Get index in `for..of`
```js
for (const [i, value] of arr.entries()) {
  indexMap[i] = value
}
```

