
A proxy object is an object that wraps another object. The proxy object intercepts the fundamental operations of the wrapped object, including property lookup, assignment, function invocations etc
	- In other words, we can have a regular object that we interact with normally. We can also wrap that regular object with a proxy object, that will intercept those interactions that we have with the object.
- Think of a proxy object like an enhanced (wrapped) component in React. It can do everything that the original can, but has some added functionality. This added functionality might be as simple as logging to the console every time a property is read (this would be implemented using a **get trap**. 
- The wrapped object is called the *target*
- The custom functionality that is added is called the *handler*
- *trap* - a method defined on the proxy that will intercept some interaction we have with the object (ie. read, write)
