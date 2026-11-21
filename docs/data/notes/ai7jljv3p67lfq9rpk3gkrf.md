
JavaScript executes all operations on a single thread, but it gives the illusion of multi-threading through the use of a few sophisticated data structures

By default, in Javascript everything is synchronous and blocking
- Callbacks/await/async/promises are all similar ways of dealing with the same problem: getting your code to wait for something without blocking everything else.