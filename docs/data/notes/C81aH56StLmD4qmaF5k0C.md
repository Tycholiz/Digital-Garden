
Map is a collection of keyed data items, just like an Object. But the main difference is that Map allows keys of any type, whereas object only allows strings and numbers.
- ex. with a Map, `true`/`false` can be boolean keys. Even an object can be used as a key
    - Using objects as keys is one of the most notable and important Map features.

Methods and properties are:
- new Map() – creates the map.
- map.set(key, value) – stores the value by the key.
- map.get(key) – returns the value by the key, undefined if key doesn’t exist in map.
- map.has(key) – returns true if the key exists, false otherwise.
- map.delete(key) – removes the value by the key.
- map.clear() – removes everything from the map.
- map.size – returns the current element count.

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
