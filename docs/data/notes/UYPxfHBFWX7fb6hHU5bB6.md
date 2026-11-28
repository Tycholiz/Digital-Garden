
`__proto` is the connection between object instances and the prototypes that they "inherit" from

Even though it is a single threaded language, JavaScript achieves concurrency (essentially) by being able to context switch well. It achieves this multitasking ability through the task queue, which is used by the event loop to determine which tasks will be sent to the call stack, and in what order. 

* * *

## Modules
The general pattern of a module is a function that defines private variables and functions; creates privileged functions which, through closure, will have access to the private variables and functions; and that returns the privileged functions or stores them in an accessible place.

# UE Resources
- [State of JS](https://2021.stateofjs.com/en-us/)
- [Dan Abramov JS Fundamentals course: highly recommended](https://justjavascript.com/)