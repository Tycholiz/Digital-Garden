
An iterator gives us the ability to say "get me the next item in the list", without us having to know anything about the underlying implementation (ie. whether it's a list, stack, tree etc.). 
- Therefore, the type of list is immaterial (ie. could be an array, linked list, object etc.)

The iterator is what hands us the next item in the sequence.

The iterator manages the state of which element of the list we are currently on (known as the `Head`). Because of this `Head`, an iterator is fully aware of its location.
- More complex iterators may let us move forwards and backwards, but they all must satisfy this simple requirement of knowing where it is.

Since a container is dumb, we need a way to traverse over elements of a collection without going over the same ones over and over.
- if our collection was based on a list, this might be a simple solution, but this problem gets more complex as we use more complex data structures
	- ex. imagine we had a tree and by default we used depth-first traversal. To traverse this, our class implements a `next()` function which gets the next item. Now imagine that something then required us to use breadth-first. Our implementation details of the `next()` function would have to change, and we would wind up muddying up our class with different traversal algorithms. Since different types of collections have different ways of accessing their elements, our only choice would be to couple our code to the specific collection class.

The purpose of an iterator is to extract the logic of how to traverse a collection into a separate object.
- We do this by defining different *iterators* (e.g. `DepthFirstIterator`, `BreadthFirstIterator`) which each implement their own methods (e.g. `next()`), with all iterators providing the same interface.
- this way, if we need to change the way that our collection is traversed, we just need to use a different iterator. No other areas of the code need to be touched.

An *iterable* is a collection that we can use an *iterator* on.

Iterators implement the *open/closed principle* since using them enables us to change how we traverse our collections / change the collections themselves, without having to change any other consuming code.

## Resources
- [Iterator in Typescript](https://refactoring.guru/design-patterns/iterator/typescript/example)