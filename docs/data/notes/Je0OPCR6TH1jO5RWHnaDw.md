
#### Execute array of callbacks
```js
const iterate = (count, callback) =>
  [...Array(count)].map((_, i) => callback(i))
```

#### Generate empty array
```js
const emptyArray = Array(5).fill(null)
```

#### Surgically remove one item from an array
```js
    return [
        ...bigArray.slice(0, itemToRemoveIndex),
        ...bigArray.slice(itemToRemoveIndex + 1)
    ]
```

#### Surgically replace one item in an array
```js
    return [
    ...bigArray.slice(0, itemToReplaceIndex),
        itemToInsert //if item is not object
        { ...array[itemToReplaceIndex], ...itemToInsert } //if item is object
        ...bigArray.slice(itemToReplaceIndex + 1)
    ]
```

#### Move elements within array
```js
function swapInArrayByIndex(arr, i1, i2){
    let t = arr[i1];
    arr[i1] = arr[i2];
    arr[i2] = t;
}

function moveBefore(arr, el){
    let ind = arr.indexOf(el);
    if(ind !== -1 && ind !== 0){
        swapInArray(arr, ind, ind - 1);
    }
}

function moveAfter(arr, el){
    let ind = arr.indexOf(el);
    if(ind !== -1 && ind !== arr.length - 1){
        swapInArray(arr, ind + 1, ind);
    }
}
```
