
Think of a generic as a "type which takes in a parameter"
- contrast this with the more familiar "function which takes in a parameter"

Think of a generically typed function as a prototype. When you define a generic `map()` function, it is basically saying "I don't care if you pass in an array of strings, or integers, or objects... just pass it in, and I will make sure the function works with that type". Conceptually, this is like creating many different versions of a function with different type parameters.

As exemplified by `map`, functions that take arrays as parameters are often generic functions. If you look at the typings for the lodash library, you will see that nearly all of them are typed as generic functions. Such functions are only interested in the fact that the argument is an array, they don’t care about the type of its elements.

When you call a function, you pass it arguments. You can think of a generic as an additional thing you pass to the function. But instead of passing a value, you pass a type to it.
- This type is inferred through whatever the type actually is during execution.

Consider the `identity` function, which most fundamentally explains the value of generics
```ts
function identity<T>(arg: T): T {
    return arg
}
```

We can describe the above function as "the generic function `identity` takes a type parameter `T`, and an argument `arg` (which is of type T), and returns a value of type `T`.

To write an algorithm generically is to think of the algorithm in terms of *types-to-be-specified-later*. When we want to use these algorithms, the arguments that we pass *inform* the generic function, and the algorithm's behavior will be tailored.
- this approach permits writing common functions or types that differ only in the set of types on which they operate when used.

What they are - When you declare a generic, E, for example. You are basically saying *"I don't know what this will be, it can be anything, so I shall simply refer to it as "the thing" and your type is a generic type."*

situation: you have some chunk of code that is identical between 2 functions, yet the function returns 2 different types. for instance, we have a createpizza and createhamburger. there is naturally a lot of overlap between these, but we can’t simply use one function between them because one function returns an object of type pizza and the other returns an object of type hamburger. to solve this, we can use a generic, and define a function called makefood:
```ts
function makefood<typeoffood>(): typeoffood {
    // do all stuff to make food
    const madefood = {}
    return madefood
}
```

a generic is kind of like a function that returns a type, and that type that is returned is modified by whatever type was “passed in”

Ex imagine we have a wrapper around fetch(). In the context of our application, fetch might return Order, Customer, Product etc. 