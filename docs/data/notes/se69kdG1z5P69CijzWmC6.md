

A generator can be used to control the iteration behaviour of a loop
A generator is also an iterator, meaning it can be looped over

A generator is very similar to a function that returns an array. A generator: 
- has parameters 
- can be called 
- generates a sequence of values

However, instead of building an array and returning all values at once, a generator yields the values one at a time, requiring less memory, and has the added benefit of allowing the caller to access the first few values immediately, instead of having to wait for the whole array to return.
- In short, a generator looks like a function but behaves like an iterator.
