
composing two functions and then mapping the resulting function over a functor should be the same as first mapping one function over the functor and then mapping the other one.
- ie. if you plan to map over the same array multiple times, just compose the functions and then map over the array once with that composed function
```js
["1", "2", "3"].map(r.unary(parseint)
```
the data flow is: 
```
arrayvalue <-- map <-- unary <-- parseint
```
(p.s., any time you see data flow presented this way, think `r.compose()`

`parseint` is the input to `unary(..)`. the output of `unary(..)` is the input to `map(..)`. the output of `map(..)` is `arrayvalue`. this is the composition of map(..) and unary(..).

having a series of functions whose input is the output provided by the previous (inner) function

![](/assets/images/2021-03-09-09-34-17.png)

encapsulating a series of functions within one function
![](/assets/images/2021-03-09-09-34-28.png)

functions compose from right to left (incl. `r.compose()`)

- expl. this is consistent with how we evaluate functions, from inner to outer (ie. right to left)
ex:
```js
["1", "2", "3"].map(r.unary(parseint)
```

composition is the wrapper around the big machine, whose contents are the parameters of `r.compose()`. in other words, the components of the machine (under the wrapper) are the individual functions that the origvalue goes through (as an argument). the composed machine returns a new function, so it is in essence a function factory.
rightmost functions are at the top of the machine.
![](/assets/images/2021-03-09-09-34-44.png)
