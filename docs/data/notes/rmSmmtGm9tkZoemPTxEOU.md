
When you create a new click handler on an HTML element, that click handler is being registered with the event loop, meaning that the event loop is now listening for that handler to be called. Every time it witnesses the event to happen, it causes a certain snippet of code to be executed.

An event loop must exist because of the fact Javascript is an asynchronous language. What happens is that we put what we need to do in the queue (eg. fetch data), and tell it "when you're finished, do this". The result is non-blocking code.

The event loop is a [[thread|os.thread]]. Javascript only has a single thread which listens for events and executes user specified functions when the event occurs.
- although the application appears to run on a single thread from the programmer's perspective, the runtime internally uses multiple threads to handle tasks. The main difference is that the programmer does not have to deal with these internal threads and the challenges of coordination between them. All the programmer has to do is specify callback functions to be executed on the main thread when those background tasks have completed.

# UE Resources
- [High quality recommended resource for understanding Event Loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
