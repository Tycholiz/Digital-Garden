
A `Set` is similar to an array, in that it is a linear non-keyed data structure. 
- Also, like an array the order of data is maintained. 
- Unlike an array, each value in the set may only occur once, making it more like an enum in that sense.

Its main methods are:
- `new Set(iterable)` – creates the set, and if an iterable object is provided (usually an array), copies values from it into the set.
- `set.add(value)` – adds a value, returns the set itself.
- `set.delete(value)` – removes the value, returns true if value existed at the moment of the call, otherwise false.
- `set.has(value)` – returns true if the value exists in the set, otherwise false.
    - `set.has` is on average faster than the `Array.prototype.includes` method when an Array object has a length equal to a Set object’s size.
- `set.clear()` – removes everything from the set.
- `set.size` – is the elements count.

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

### Why and when to use Sets
- Eliminating Duplicates: Sets are commonly used to remove duplicate values from an array or collection. By converting an array to a Set and then back to an array, you can easily eliminate any duplicate values.
- Filtering Unique Values: Sets can be used to filter unique values from an array or collection. By adding elements to a Set, you automatically eliminate duplicates, allowing you to obtain a collection of unique values.
    - tip: if you think in terms of [[math.set-theory]], each element of the set would be what we add to the `Set`. In this example, we are making a `Set` of conversations that satisfy a certain condition. Imagining a venn diagram, one circle would be composed of elements that satisfy this condition, while the second circle would be composed of elements that don't satisfy this condition. In both cases, they are conversations, so the `conversationId` is what we should build the set on.
```ts
// return a Set of conversationIds that doesn't include duplicates in cases where multiple messages are associated with a single conversationId
const conversationsWhereBusinessResponded: Set<number> = allMessages.reduce((conversations: Set<number>, message: Message) => {
    if (message.sender === bizOwnerId) {
        conversations.add(message.conversationId)
    }
    return conversations
}, new Set())
```
- Iterating Over Unique Elements: Sets maintain the order of inserted elements, which makes them useful for iterating (e.g. with `forEach()`) over unique elements in the order of insertion.
- Membership Testing: Sets are efficient for checking whether a particular element exists in a collection. The has() method of a Set can quickly determine if an element is present or not, without the need for iterating through the entire collection.
- Operations Based on Set Theory: Sets can be used to perform set operations such as union, intersection, and difference. These operations can be useful when dealing with collections that need to be combined, compared, or filtered based on their elements.
- Checking Array Similarity: Sets can be used to determine the similarity or differences between two arrays. By converting both arrays to Sets, you can easily perform set operations to identify common or distinct elements.
- Tracking Unique Events: Sets can be employed to keep track of unique events or triggers. As events occur, you can add them to a Set, ensuring that only unique events are stored.