
## Purifying functions
- Use immutable data structures (ex. Immutable.js)
- Sometimes you can just shift the side effects out of a function to the part of the program where the call of that function happens. The side effect wasn't eliminated, but it was made more obvious by showing up at the call-site.

```js
function addMaxNum(arr) {
    var maxNum = Math.max( ...arr );
    arr.push( maxNum + 1 );
}

var nums = [4,2,7,3];

addMaxNum( nums );

nums;       // [4,2,7,3,8]
```
to

```js
function addMaxNum(arr) {
    var maxNum = Math.max( ...arr );
    arr.push( maxNum + 1 );
}

var nums = [4,2,7,3];

addMaxNum( nums );

nums;       // [4,2,7,3,8]
```
