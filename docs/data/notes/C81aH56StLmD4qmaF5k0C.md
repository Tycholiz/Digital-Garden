
A `Map` is a collection of keyed data items, just like an Object. But the main difference is that Map allows keys of any type (including objects, functions, strings, and symbols), whereas object only allows strings and numbers.
- ex. with a Map, `true`/`false` can be boolean keys. Even an object can be used as a key
    - Using objects as keys is one of the most notable and important Map features.

Maps perform better than plain Objects in scenarios involving frequent additions and removals of key-value pairs.

Methods and properties are:
- `new Map()` – creates the map.
- `map.set(key, value)` – stores the value by the key.
- `map.get(key)` – returns the value by the key, undefined if key doesn’t exist in map.
- `map.has(key)` – returns true if the key exists, false otherwise.
- `map.delete(key)` – removes the value by the key.
- `map.clear()` – removes everything from the map.
- `map.size` – returns the current element count.

```js
const myMap = newMap()

myMap.set('a', 1)
myMap.set('b', 2)
myMap.get('a') // 1

/*
{
    a: 1,
    b: 2
}
*/
```

### Why and when to use Maps
- Key Type Flexibility: Maps allow keys of any type, including objects, functions, strings, and symbols. This flexibility can be beneficial when you need to associate values with complex or non-primitive keys that may not be easily represented as strings.
- Preserving Key Order: Maps maintain the order of key-value pairs based on the insertion order. This is particularly useful when you need to iterate over the entries in the same order as they were added. Regular objects do not guarantee a specific order for their properties.
- Handling Large Key Sets: Maps are more efficient when dealing with a large number of key-value pairs. The performance of Map operations, such as retrieval and deletion, remains consistent regardless of the size of the map. In contrast, objects may experience performance degradation when the number of properties becomes very large.
- Iterating Over Key-Value Pairs: Maps provide built-in methods such as `forEach()` and `for...of` loops to iterate over the key-value pairs directly. This simplifies operations that require iteration, transformation, or computation based on each entry in the map.
- Easy Removal of Entries: Maps offer a straightforward way to remove entries from the collection using the delete() method. With regular objects, removing properties requires more explicit checks and assignments.