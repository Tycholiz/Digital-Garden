
**kwargs** - similar to `...args` in javascript

### Self
- a reference to the current instance of the class
- `self` is always the first argument of a class method (including the `init` method)
    - In the `init` method, `self` refers to the newly created object
    - In other class methods, it refers to the instance whose method was called.

### .init
- it is a constructor
- Called when an object is instantiated.
- It will initialize the attributes of the class

### super
- give the current object access to the methods from its superclass
- `super` returns a temporary object of the superclass, which allows us to call that superclass's methods
- Common use case
    - building classes that extend functionality of previously built classes
  
# Celery
- Async task queueing system that can allow you to queue up jobs at schedules, or have them queue up on a trigger (ex. on some get request).
