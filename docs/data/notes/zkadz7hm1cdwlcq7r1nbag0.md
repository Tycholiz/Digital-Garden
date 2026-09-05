
### Union ($\cup$)
Get elements that are in either array
- see [[union Dendron node|math.set-theory.op.union]]
```js
function getUnion(arr1, arr2) {
  const mergedArray = arr1.concat(arr2);
  const unionSet = new Set(mergedArray);
  return Array.from(unionSet);
}
```

### Intersection ($\cap$)
Get elements that both arrays share in common
- see [[intersection Dendron node|math.set-theory.op.intersection]]

```js
let intersection = arrA.filter(x => arrB.includes(x))
```

### Difference
Get elements from `arrA` that are not in `arrB`
```js
let difference = arrA.filter(x => !arrB.includes(x));
```

### Symmetrical Difference
Get elements that are in `arrA` but not in `arrB`, *and* elements that are in `arrB` but not in `arrB`
```js
let difference = arrA
    .filter(x => !arrB.includes(x))
    .concat(arrB.filter(x => !arrA.includes(x)));
```