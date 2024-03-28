
When calling reduce you specify a function that describes how the current array element will alter the accumulator. 
- It's like a functional version of a forEach loop with an additional variable that you change in the loop and return at the end.

The `reduce()` method executes a user-supplied “reducer” callback function on each element of the array, 
- the return value of the calculation on the previous element (the "accumulator") gets passed as input. 

`reduce` could just as easily have been named `transform`, for its ability to transform one data structure into another.

Anytime you need to do filter and map on same array, use reduce

Anytime you need to use `reduce` to get uniqueness out of an array of objects, use `Set`

Anytime you need to skip over an iteration of the reducer function, simply return the accumulator

The final result of running the reducer across all elements of the array is a single value (whether that single value is an array, object, integer etc.).

The accumulator (ie. reducer function) is the hero of `.reduce()`. It is a story about how the accumulator changes at each step of the way (ie. at each array element). The way that the accumulator changes is described by the reducer function. The value returned from that reducer function describes what that accumulator looks like at that particular iteration.
- ex. Take for instance a reducer function that adds up all elements in the array. The accumulator starts out as the array's first element (assuming no `defaultValue` passed). As we iterate through the array, the Accumulator is modified in a way that is described by the return value of the reducer function. Whatever value this function returns becomes the value of the Accumulator for that iteration.

A reducer function takes the `accumulator` and `currentValue` arguments.
- *accumulator* - the value resulting from the previous call to the callback.
- *currentValue* - the value of the current element

### `initialValue`
If initialValue is specified, that also causes `currentValue` to be initialized to the first value in the array. 
If initialValue is not specified, `previousValue` is initialized to the first value in the array, and currentValue is initialized to the second value in the array.

### Signature
Signature
```ts
function reduce<Element, ReturnType>(arr: Element[], initialValue?: ReturnType) {
    (acc: ReturnType, val: Element) => ReturnType
}

```
