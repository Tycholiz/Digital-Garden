
## What are they?
A decorator is a function that can be used to extend (decorate) the functionality of a certain object at run-time, independently of other instances of the same class.
- This is achieved by designing a new Decorator class that wraps the original class/method.
- One value is that they read more like plain English, and allow us to discern at a distance what behaviour has been added to a given class/method.

Decorators are helpful for anything you want to transparently wrap with extra functionality. 
- ex. memoization, enforcing access control and authentication, instrumentation and timing functions, logging, rate-limiting etc.

Decorators help us adhere to Single Responsibility Principle, since it allows functionality to be divided between classes with unique areas of concern.
- We might have solved this problem with [[subclassing|paradigm.oop.inheritance]], but decorators can be more efficient because an object's behavior can be augmented without defining an entirely new object.
    - subclassing (ie. `extend`ing) happens at compile time, meaning that this binding cannot be changed at run-time.
  
### Differences between other structural patterns
While an [[Adapter|general.patterns.structural.adapter]] provides a different interface to its subject, and a Proxy provides the same interface, a Decorator provides an enhanced interface.
- a Decorator enhances an object's responsibilities. Decorator is thus more transparent to the client. As a consequence, Decorator supports recursive composition, which isn't possible with pure Adapters.

An [[adapter|general.patterns.structural.adapter]] should be used when the wrapper must respect a particular interface and must support [[polymorphic|paradigm.oop.polymorphism]] behavior.

Decorator and Proxy have different purposes but similar structures. Both describe how to provide a level of indirection to another object, and the implementations keep a reference to the object to which they forward requests.


## How are they used?
Decorators can be thought of as functions.
- you can think of:
  ```ts
  @Decorator
  class Foo {}
  ```
as
  ```ts
  Decorator( new Foo() )
  ```

Imagine having a `Decorator` class that implements the interface of the extended (decorated) object transparently by forwarding all requests to it 

### Example
Imagine that we had a `@UserValidatorDecorator` whose sole responsibility was to verify that a username adheres to our application's business logic:
- note: not valid code; just for illustrative purposes
```ts
function UserValidatorDecorator(user) {
  if (user.username === "") {
    throw new Error('Invalid!')
  }
}
```
Then, we can use the decorator to ensure that each time we instantiate, the logic of the decorator is used:
```ts
@UserValidatorDecorator
class User {
  username: string = ""
}
```

### Decorator Factory
A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime.

Example:
```ts
function color(value: string) {
  // this is the decorator factory. it sets up the returned decorator function
  return function (target) {
    // this is the decorator. do something with 'target' and 'value'...
  };
}
```

### Alternatives to Decorators
