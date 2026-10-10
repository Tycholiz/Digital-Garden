
`this` only cares about execution context (where the fn was called; ie. call-site). it doesn't care about the scope chain

`this` is essentially an implicit input to a function, thereby negatively impacting function purity. Think of `this` as an implicit argument that gets passed into the function.
- ie. because of this impure nature, `this` shouldn't be used in [[Functional Programming|paradigm.functional]]

`this` is context-bound and not instance-bound. If, for example, a method is passed on as a callback, it loses its context. If the method is then called like a function, the context will be undefined. As such, in order to avoid this scenario, the `this` context has to be captured in the method. This can be done either by wrapping the method (() => f()), using a field with an arrow function instead (f = () => {}), or using a bound version of it using bind (f = f.bind(`this`)).

The scope chain encompasses the prototype chain
- Expl. Imagine 2 functions: inner() and outer(). If a variable is used within inner() and it does not exist within that function's context, it will look at its prototype to see if it exists (prototype chain). If it goes all the way up the chain and still doesn't exist, then the scope of outer() will be considered.
- Expl. If this were illustrated as 2 for loops, the scope chain would be the `i` iterator, while prototype would be `j`

`return` is a keyword which returns us to the immediate outer execution context (to continue parsing at the point directly after where the function was called)

## Classes
The behavior of `this` in classes and functions is similar, since classes are functions under the hood. There are some important caveats:
- methods of a class can be accessed on `this`.
	- We might assume that this means that the method is a property on the `this` object. This is a false assumption. The reality is that the method is a property on the class itself (which is the prototype of `this`). This means that when we are accessing a method like `this.moveUnit`, the `moveUnit` is being accessed by traversing this prototype chain, up from `this` and reaching the class itself, upon which the method is found.
- if a method is marked `static`, then it is not added to the prototype of `this`.

## Arrow functions
Until arrow functions, every new function defined its own `this` value (a new object in the case of a constructor, `undefined` in strict mode function calls, the base object if the function is called as an "object method", etc.). 
- This proved to be less than ideal with an object-oriented style of programming.

An arrow function does not have its own `this`; the `this` value of the enclosing execution context is used.

In arrow functions, `this` retains the value of the enclosing lexical context's `this`, whereas a normal function's `this` value is whatever the caller's scope is.
- In global code, it will be set to the global object. Here, the enclosing lexical context (ie. the context of `foo`) is the global context:
```js
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true
// `true` returned because the function `foo` is in the global scope
```

This logic follows that if we had an arrow function within another function, the arrow functions `this` value would retain the value of the outer functions `this` object.
```js
function outer() {
	const outerFunctionContext = this
	const inner = (() => this)
	console.log(outerFunctionContext === inner())
}
```
