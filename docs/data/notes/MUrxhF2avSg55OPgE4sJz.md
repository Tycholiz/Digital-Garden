
When calling reduce you specify a function that describes how to calculate the result for the next array element, given you already know the result for the previous array elements. 
- It’s like a functional version of a foreach loop with an additional variable that you change in the loop and return at the end.

The `reduce()` method executes a user-supplied “reducer” callback function on each element of the array, 
- the return value of the calculation on the previous element gets passed as input. Therefore, each calculation feeds the next element during the next iteration.

`reduce` could just as easily have been named `transform`, for its ability to transform one data structure into another.

Anytime you need to do filter and map on same array, use reduce

The final result of running the reducer across all elements of the array is a single value.

The accumulator is the hero of `.reduce()`. It is a story about how the accumulator changes at each step of the way (ie. at each array element). The way that the accumulator changes is described by the reducer function. The value returned from that reducer function describes what that accumulator looks like at that particular iteration.
- ex. Take for instance a reducer function that adds up all elements in the array. The hero of the story (ie. the Accumulator) starts out as the array's first element (assuming no `defaultValue` passed). As we iterate through the array, the Accumulator is modified in a way that is described by the return value of the reducer function. Whatever value this function returns becomes the value of the Accumulator for that iteration.

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
