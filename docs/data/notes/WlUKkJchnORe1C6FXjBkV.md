
#### Pick random element in array
```js
function randomPick(array) {
  let choice = Math.floor(Math.random() * array.length);
  return array[choice];
}
```

#### Generate random number between 1-10
```js
Math.floor(Math.random() * 10) + 1
```
