
- can be exited and later re-entered
- like closures, variables inside the generator function maintain state.
- when calling a generator function, an iterator object is returned. When we call `next()` on that object, all the code up until the first `yield` will be executed. Calling `next()` again will then execute all the code up until the second `yield`, and so on.
	- The function that calls the generator function is the **iterator**
- the generator function can pass values to the iterator object (`yield`). Anything that occurs after `yield` gets stored in the iterator's `next()` value
	- The generator function can also retrieve values from the iterator object (`next(___)`)
- `yield` returns execution to outside the generator function (ie. the context from which the gen fn was called), it's possible to use `while(true)`, as long as there is a yield inside
	- This way, `next()` can keep getting called
- spec: `next` is like async/await in the sense that it will execute code up until a point (`yield`), then stop and wait for the availability of that data before continuing on

# UE Resources
[Observable Async flow control (Eric Elliott)](https://medium.com/javascript-scene/the-hidden-power-of-es6-generators-observable-async-flow-control-cfa4c7f31435)
