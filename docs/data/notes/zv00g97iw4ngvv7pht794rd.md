
If [[composition|paradigm.oop.composition]] is about *using* another class/interface, inheritance is about *being* one.

Inheritance is most useful for 
- grouping related sets of concepts
- identifying families of classes
- organizing the names and concepts that describe the domain

## How to use inheritance
Inheritance is best suited for *differential programming*, whereby we are making something that could be considered a variation of something else. That is, the new thing has some tweaks and enhancements of the original thing.
- this is appropriate, since when we instantiate the new class, we still want to preserve the interface of the original class.

Inheritance should only be used when:
- Both classes are in the same logical [[domain|general.principles.DDD]]
- The subclass is a proper subtype of the superclass
- The superclass’s implementation is necessary or appropriate for the subclass
- The enhancements made by the subclass are primarily additive.


## How to misuse inheritance
Inheritance is prone to misuse.

### Inheritance gives us the properties / methods of its ancestors
Consider the class
```js
class Stack extends ArrayList {
  public void push(value: Object) { … }
  public pop(): Object { … }
}
```

The interface of an instance of this class is bloated. It's reasonable to expect an interface of the class `Stack` to have only `push` and `pop`, but it also includes `get`, `set`, `add`, `remove`, `clear`, among others.

In carrying out this pattern, we committed 3 mistakes:
- By inheriting, we implicitly acknowledged that "a Stack is an ArrayList". However, this is not true, since `Stack` is [[not a proper subtype|paradigm.oop.substitutability]] of `ArrayList`, because a `Stack` is supposed to enforce *last-in-first-out*. Yes, the `push` / `pop` interface supports that, but the fields exposed by `ArrayList` violate that.
- By inheriting, we are violating [[paradigm.oop.encapsulation]], since we use `ArrayList` to hold the stack’s object collection. This is an implementation choice that should be hidden from consumers.
- By inheriting, we are creating a cross-domain relationship. There are 2 different concepts at play here: a randomly-accessible collection (ArrayList), and a [[queue|general.lang.data-structs.queue]] (Stack)

### Mixing Domain classes and Implementation classes
Imagine we wanted to make a variable that represented a subset of customers. Our instinct might be to inherit from `ArrayList` like so:
```js
class CustomerList extends ArrayList {}
```

The problem here is that `CustomerList` is a domain class, while `ArrayList` is an implementation class. Anything from the implementation layer should be invisible at the domain layer. Instead, domain classes should *use* implementation classes, not *inherit* from them.
- *domain* - what our software does (some relation to business logic)
- *implementation* - how our software works

Instead, we should take an approach where we:
1. create implementation classes (ie. our mechanical, non-domain structures) by inheriting from utility classes
2. use these mechanical structures in our domain classes via [[composition|paradigm.oop.composition]], rather than inheritance. In other words, we should not have our domain classes inherit from implementation classes.

## E Resources
- [Composition vs Inheritance](https://www.thoughtworks.com/en-ca/insights/blog/composition-vs-inheritance-how-choose)