
A Javascript [[decorator|general.patterns.structural.decorators]] is an expression which returns a function and can take a `target`, `name` and `property descriptor` as arguments. 
- You apply it by prefixing the decorator with an `@` character and placing this at the very top of what you are trying to decorate. 
- Decorators can be defined for either a class or property.

Decorators get called first before classes are compiled.
- In Javascript when we define a new `class` with methods, we are really just installing a descriptor on the object's prototype.

A decorator is just an expression that will be evaluated and has to return a function.

### Readonly decorator
```js
function readonly(target, key, descriptor) {
  descriptor.writable = false
  return descriptor
}

class Horse {
  @readonly
  neigh() console.log('*neigh!*')
}
```

### Class decorators
`@decorator` is a function that returns a decorator.
- a decorator itself is a function that takes in a class and returns that same class.
