
# Push vs Pull data creation
- Pull and Push are two different protocols that describe how a data Producer can communicate with a data Consumer.

|      | Producer                              | Consumer                               |
|------|---------------------------------------|----------------------------------------|
| Pull | Passive: produces data when requested | Active: decides when data is requested |
| Push | Active: produces data at its own pace | Passive: reacts to received data       |

Note:
- A regular function is a lazily evaluated computation that synchronously returns a single value on invocation.
- A generator function is a lazily evaluated computation that synchronously returns zero to (potentially) infinite values on iteration.
- A Promise is a computation that may (or may not) eventually return a single value.
- An Observable is a lazily evaluated computation that can synchronously or asynchronously return zero to (potentially) infinite values from the time it's invoked onwards.

**Pull**
- Consumer determines when it receives data from the data Producer
- The Producer itself is unaware of when the data will be delivered to the Consumer.
- ex. every javascript function is a pull system
	- The function itself is the producer, and the calling code is the consumer. The reason the calling code is called the consumer is because the calling code "pulls" out a *single* return value
- ex. React is a pull system. When React needs to re-render, it will call the render function of every affected component. This will return a new representation of the UI, which React can reconcile with the previous one. Any changes are then propagated to the DOM.

**Push**
- Producer determines when to send data to the consumer, and the consumer is unaware of when it will receive that data.
	- called push because now the producer of the state is responsible for handing the new value over to those that depend on it.
	- This has a positive effect: only those entities that depend on the value that has changed will update, and it can be done without having to make comparisons or detect changes.
- ex. Promises and Observables are a push system, since the promise (a producer) delivers a resolved value to the registered callbacks (the consumers).
	- it is the Promise which is in charge of determining precisely when that value is "pushed" to the callbacks.
	- ex. An Observable is a Producer of multiple values, "pushing" them to Observers (Consumers).
- ex. [[RxJS|rxjs]] uses a push-based approach, where you declaratively define streams and their relationships, and RxJS will propagate every change from one stream to the next one.
