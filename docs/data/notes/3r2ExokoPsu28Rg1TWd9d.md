
### Access modifiers
- public
- private
- internal
- `protected` 
    - only inherited classes can access this method

### Abstract classes
Abstract classes cannot be instantiated, but they can be subclassed.
When an abstract class is subclassed, the subclass usually provides implementations for all of the abstract methods in its parent class. However, if it does not, then the subclass must also be declared abstract.

Abstract classes are for:
- code sharing among related classes
- we expect the classes that extend from the abstract classes to have a huge overlap of fields/methods.

### Static method
a static method is part of class, but we do not need to create a instance to call it
- because static methods do not belong to an object, they therefore cannot be used to access any variables that belong to an object.
- contrasted with *instance methods*, which must belong to an instance of a class.