
Currying is the process of transforming a function of N arity (num of args) into N functions of 1 arity.
- ex. a function with 4 arguments -> 4 functions with 1 argument each

### Partial application
Applying a certain number of args toward completion of the function (when the last argument is called)

Application === calling a function and applying it's return value

JavaScript engine does its job in two phases: memory creation (declaring variables/functions and hoisting them), and execution (initializing variables and actually running through code)

## Why use these techniques?
The first and most obvious reason is that both currying and partial application allow you to separate in time/space (throughout your codebase) when and where separate arguments are specified, whereas traditional function calls require all the arguments to be present at the same time. If you have a place in your code where you'll know some of the arguments and another place where the other arguments are determined, currying or partial application are very useful.

Another layer to this answer, specifically for currying, is that composition of functions is much easier when there's only one argument. So a function that ultimately needs three arguments, if curried, becomes a function that needs just one, three times over. That kind of unary function will be a lot easier to work with when we start composing them. But the most important layer is specialization of generalized functions, and how such abstraction improves readability of code.

R.partial says "you give me a function and any number of arguments you want, then I'll just keep letting you add in as many arguments as you want either indefinitely or until all parameters of the function have been satisfied with arguments"
![](:/7548f55bd87c4c279f38a15ebac02ed5)

### Partial application of composed functions
allows us to compose multiple functions together, while returning a function that will accept more. This has the benefit of allowing us to compose more and more specific functions.
expl. `unique` and `words` will get partially applied to `compose`

```js
const filterWords = partialRight( compose, unique, words )
const biggerWords = filterWords(skipShortWords)
```

![Untitled Diagram.jpg](:/593b0123095a48c98e17db790864665f)

### Currying composed functions
since compose has right-to-left ordering, we normally `R.curry(R.reverseArgs(R.compose), ..)`

*Using partials with `.pipe()`*
`var filterWords = partial( pipe, words, unique )`

#### Functor
something that can be mapped over
ex. array, object
- a functor that holds values of type A, when mapped over with a function that takes a value of type A and returns a value of type B, the result must be a functor that holds values of type B

#### Applicative
a subtype of functors for which additional functions are defined.
[more info](https://medium.com/@JosephJnk/an-introduction-to-applicative-functors-aea966799b1d)
