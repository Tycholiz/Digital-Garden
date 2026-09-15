
Use `flatMap` any time you have a [[js.lang.array.method.map]]-type problem, and think "on certain (or all) iterations, I need to map one element to multiple elements".
- thought of another way, any time our map-type problem has us desiring to output an array that is of greater length than the input array.

```js
const arr1 = [1, 2, 1];

const result = arr1.flatMap((num) => (num === 2 ? [2, 2] : 1));

console.log(result);
// Expected output: Array [1, 2, 2, 1]
```