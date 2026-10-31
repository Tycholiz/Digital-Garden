
The concurrency model in Javascript is based on the Event Loop, and it works on a single thread.
- Recall: JavaScript is a synchronous programming language.

The Event Loop is a watchdog that ensures that the Call Stack and Callback Queue are in constant communication. 
- It is responsible for executing the code, collecting and processing events, and executing queued sub-tasks.
- It first determines whether the call stack is free, and then informs the user of the callback queue.
- The callback function is then passed to the Call stack, which executes it. 
- The call stack is out and the global execution context is free once all of the callback functions have been performed.

The runtime model of JavaScript is based on an event loop, which executes the code, collects and processes events, and executes queued sub-tasks.

Javascript performs task execution by dividing them into “microtasks” and “macrotasks.”

Within the Event Loop, there are 2 type of queues: 
1. the (macro)task queue (or just called the task queue)
2. the microtask queue.

### Microtask
If the Javascript execution stack is empty by the time a program exits, a microtask is executed.
- a microtask is just a short function

Before any other event handling, rendering, or “macrotask” occurs, all microtasks are performed. 
- This is significant because it ensures that the application environment remains essentially the same (no changes in mouse coordinates, no new network data, etc.) between microtasks.

We can use `window.queueMicrotask` to plan a function to run asynchronously but before updates are rendered or new events are handled.

When a Promise resolves and calls its `then()`, `catch()` or `finally()`, method, the callback within the method gets added to the microtask queue

### Macrotask
Once the Javascript Execution Stack and Microtask queue are empty, a short function (a macrotask) is executed.
- a macrotask is also just a short function
- ex. setTimeout, setInterval, setImmediate

In a single macrotask execution cycle, all logged microtasks are processed in one fell swoop. The macrotask queue, on the other hand, has a lower priority. Parsing HTML, producing DOM, executing main thread JavaScript code, and other events like page loading, input, network events, timer events, and so on are all macrotasks.

### How it works
When we invoke a function, it gets added to the call stack. When the function returns a value, it gets popped off the stack
- remember, async functions (ie. functions that return promises) return immediately after exection.

When we call `setTimeout`, here's what happens:
1. A function is put onto the call stack immediately and returns immediately, thereby getting popped off the call stack.
2. The callback function that is passed to `setTimeout` gets sent to the Web API
3. In the Web API (or Node API if we're on a server), a timer runs for as long as the second argument we passed to it.
4. Then, after the specified time, it gets added to the callback queue.

When we call `fetch`, here's what happens:
1. A function is put onto the call stack immediately and returns immediately, thereby getting popped off the call stack.
2. An http request is initiated and handled by the Web API
3. Once the HTTP request completes (either successfully or with an error), the associated callback (e.g., then or catch handlers) is moved to the callback queue.
4. The event loop checks the call stack and finds it empty, then moves the callback from the callback queue to the call stack for execution.

The Event Loop's only job is to move items from the callback queue to the call stack
- If the call stack is empty, (ie. if all previously invoked functions have returned their values and have been popped off the stack), the first item in the callback queue gets added to the call stack
    - in this circumstance, no other functions had been invoked, meaning that the call stack was empty by the time the callback function was the first item in the callback queue.

As mentioned, when a Promise resolves and calls its `then()`, `catch()` or `finally()`, method, the callback within the method gets added to the microtask queue
- The implication here is that the callback within the `then()`, `catch()` or `finally()` method isn’t executed immediately, essentially adding async behavior to our code. This is where the event loop comes in.

The reason the callback isn't called right away is because the event loop gives a different priority to different tasks
- All functions currently in the call stack get executed. When they return a value, they are popped off the stack.
- When the call stack is empty, all queued up microtasks are popped onto the callstack one by one, and are executed
- If both the call stack and microtask queue are empty, the event loop checks if there are tasks left on the macrotask queue. The tasks get popped onto the callstack, executed, and popped off

### Demonstration
```ts
console.log('Start');

setTimeout(() => {
  console.log('Timeout callback');
}, 1000);

fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => {
    console.log('Fetch data:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });

console.log('End');
```

#### Execution Flow:
- console.log('Start') is pushed to the call stack and executed.
- setTimeout is called, its timer is set in the Web API, and it immediately returns.
- fetch is called, the HTTP request is initiated in the Web API, and it immediately returns a Promise.
- console.log('End') is pushed to the call stack and executed.

The main thread is now free to handle other tasks.

After 1 second, the timer set by setTimeout expires, and the callback is moved to the callback queue. Meanwhile, when the HTTP request initiated by fetch completes, its associated callback is also moved to the callback queue.

The event loop moves these callbacks to the call stack (when it’s empty) to execute them. The exact order of their execution depends on when each asynchronous operation completes.

* * *

When you create a new click handler on an HTML element, that click handler is being registered with the event loop, meaning that the event loop is now listening for that handler to be called. Every time it witnesses the event to happen, it causes a certain snippet of code to be executed.

An event loop must exist because of the fact Javascript is an asynchronous language. What happens is that we put what we need to do in the queue (eg. fetch data), and tell it "when you're finished, do this". The result is non-blocking code.

The event loop is a [[thread|os.thread]]. Javascript only has a single thread which listens for events and executes user specified functions when the event occurs.
- although the application appears to run on a single thread from the programmer's perspective, the runtime internally uses multiple threads to handle tasks. The main difference is that the programmer does not have to deal with these internal threads and the challenges of coordination between them. All the programmer has to do is specify callback functions to be executed on the main thread when those background tasks have completed.

# UE Resources
- [High quality recommended resource for understanding Event Loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
