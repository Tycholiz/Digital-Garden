
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

Getter and Setter methods should exist for all instance variables that we want to change.
- By forcing other code to go through setter methods, the setter method can validate the parameter and decide if it’s do-able
  - ex. "are negative numbers for this variable allowed?"
- Setters and Getters also unify the interface so that consumer code isn't broken
  - ex. if a consumer accessed an instance variable directly, and then the class was modified so it became `private` with an accompanied Getter (because, for example we wanted to add some logic checks), it would break everyone's code.

Encapsulation refers to the bundling of data with the methods that operate on that data, all the while restricting of direct access to some of an object's components

Encapsulation is used to hide the values or state of a structured data object inside a class, preventing direct access to them by clients in a way that could expose hidden implementation details or violate state invariance maintained by the methods.

Traditionally, JavaScript developers used `_` to prefix the properties or methods that they intended to be private.
