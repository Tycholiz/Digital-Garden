
### Convert array of objects into Object
```js
// before
const data = [
  { key: 1, name: "A", condition: true },
  { key: 4, name: "B", condition: false },
]

// after
const data = {
  1 : { key: 1, name: "A", condition: true },
  4 : { key: 4, name: "B", condition: false },
]
```

```js
const arrayToObject2 = (array, key) =>
    array.reduce((obj, item) => ({
            ...obj, [item[key]]: item
        }), {}
    )
```

### Return an object
```js
const group = (collection, grouper) => {
    return collection.reduce((acc, val) => {
        const objectIndex = grouper(val)
        if (acc[objectIndex]) {
            acc[objectIndex].push(val)
        } else {
            acc[objectIndex] = [val]
        }
        return acc
    }, {});
}

console.log(group([6.5, 4.2, 6.3, 6.8, 4, 3, 1], Math.floor))
// { '4': [4.2], '6': [6.5, 6.3] }
```

### Remove duplicates from array
This shows how we can introspect on the array/object as it's being built up
```js
const arrayWithNoDuplicates = myArray.reduce((acc, val) => {
  if (acc.indexOf(val) === -1) {
    acc.push(val)
  }
  return acc
}, [])
```

### `.map()` implemented in `.reduce()`
```js
if (!Array.prototype.mapUsingReduce) {
  Array.prototype.mapUsingReduce = function(callback, initialValue) {
    return this.reduce(function(mappedArray, currentValue, currentIndex, array) {
      mappedArray[currentIndex] = callback.call(initialValue, currentValue, currentIndex, array)
      return mappedArray
    }, [])
  }
}
```
