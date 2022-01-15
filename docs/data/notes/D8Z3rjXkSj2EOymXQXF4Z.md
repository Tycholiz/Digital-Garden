
# Push vs Pull data creation
- Pull and Push are two different protocols that describe how a data Producer can communicate with a data Consumer.
![](/assets/images/2021-03-07-22-37-12.png)

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

**Push**
- Producer determines when to send data to the consumer, and the consumer is unaware of when it will receive that data.
- ex. Promises and Observables are a push system, since the promise (a producer) delivers a resolved value to the registered callbacks (the consumers).
	- it is the Promise which is in charge of determining precisely when that value is "pushed" to the callbacks.
	- ex. An Observable is a Producer of multiple values, "pushing" them to Observers (Consumers).
