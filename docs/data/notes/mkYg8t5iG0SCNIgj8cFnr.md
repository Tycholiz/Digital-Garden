
Encapsulation is essentially about protecting the private data of an object such that they can only be accessed or mutated via the interface (ie. public API) that is exposed by that object.
- in the following example, we can only manipulate `count` by calling `increment`; we cannot directly modify it.
```js
class Counter {
  private count = 0

  public increment() {
    count++
  }
}
```

Encapsulation refers to the bundling of data with the methods that operate on that data, all the while restricting of direct access to some of an object's components

Encapsulation is used to hide the values or state of a structured data object inside a class, preventing direct access to them by clients in a way that could expose hidden implementation details or violate state invariance maintained by the methods.

Traditionally, JavaScript developers used `_` to prefix the properties or methods that they intended to be private.
