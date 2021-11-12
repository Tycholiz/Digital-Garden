
an instance of `Symbol` is guaranteed to be unique.

Symbols can be used to add unique property keys to an object that won't collide with other code that might be added to the object.
- this effectively enables a weak form of encapsulation.

Symbols are specifically created to be used as Object keys where you fear that key might be overwritten by someone else and lead to trouble (as it can happen with String keys for example).
- Since Symbols are all unique, the only way to access a property with a Symbol key is to already have that Symbol. This means you either have to be the one who wrote the code, or that Symbol was defined by the standard (e.g. `Symbol.iterator`) and they did that to avoid breaking older code that might have used `obj["iterator"]` for example.

```js
let sym1 = Symbol()
let sym2 = Symbol('foo')
// since symbols are guaranteed to be unique, we can create multiple symbols with the same description
let sym3 = Symbol('foo')

console.log(sym2 === sym3) // false
```

The `description` is the value passed to `Symbol()`.
- can be used for debugging purposes, but not to access the `symbol` itself.

# UE Resources
[Recommended MDN article on Symbols](https://hacks.mozilla.org/2015/06/es6-in-depth-symbols/)
