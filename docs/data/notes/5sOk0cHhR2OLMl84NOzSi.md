
### Splitting out a data structure into 2 forms: one for maintainability, and one for functionality
- [source: Robot Delivery game `buildGraph` function.](https://eloquentjavascript.net/07_robot.html)
In this project, it makes sense to have an object of key-value pairs 
- where the key is the name of a node, and the value is an array of adjacent nodes that can be travelled directly to (ie. without having to go through another node to get there):
```js
const graph = {
    'Alice-House': ['Bob-House', 'Post-office', 'Cabin']
    // ...
}
```
We could do this by hand, but then we are looking at a maintainability nightmare. Consider that if we add a new node that connects to 3 other existing nodes, then we need to modify the existing object in 4 places: modify 3 values (ie. the arrays), and add a new key. 
- When building out this code, we can take a strong focus to future-proofing it. Always consider likely maintenance needs, and find a way to make it simpler.

The way the project goes about future proofing this is by using a more primary way of representing the nodes, which is done through using an array where each element is an edge (ie. the link between 2 nodes):
```js
const roads = [
  "Alice's House-Bob's House",   "Alice's House-Cabin",
  // ...
]
```

a `buildGraph` function is then made, which takes in the array and outputs the `graph` object. Now, whenever a new node is added to the town, only a single element needs to be added to the `roads` array.

### Immutability in data structures
Immutable data structures help you to understand your programs, making it a solution to the problem of complexity management. When objects in a system are known to be fixed, stable things, we can consider operations in isolation (in the Robot game above, this might be moving from one location to another; going from location A to location B always produces the same new state). A simpler program lets us build it more ambitiously.
