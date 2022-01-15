
A `Set` is similar to an array, in that it is a linear non-keyed data structure. 
- Also, like an array the order of data is maintained. 
- Unlike an array, each value in the set may only occur once, making it more like an enum in that sense.

Its main methods are:
- new Set(iterable) – creates the set, and if an iterable object is provided (usually an array), copies values from it into the set.
- set.add(value) – adds a value, returns the set itself.
- set.delete(value) – removes the value, returns true if value existed at the moment of the call, otherwise false.
- set.has(value) – returns true if the value exists in the set, otherwise false.
    - `set.has` is on average faster than the `Array.prototype.includes` method when an Array object has a length equal to a Set object’s size.
- set.clear() – removes everything from the set.
- set.size – is the elements count.

A key feature is that repeated calls of `set.add(value)` with the same value don’t do anything. That’s the reason why each value appears in a Set only once.

For example, we have visitors coming, and we’d like to remember everyone. But repeated visits should not lead to duplicates. A visitor must be “counted” only once.

A set can be iterated over with `forEach` or `for..of`

```js
const mySet = new Set()
mySet.add(2)
mySet.add(5)
mySet.add(1)

// [2, 5, 1]
```
