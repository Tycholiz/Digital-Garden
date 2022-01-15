
## Tasks
When we run a task in Node (ex. with `run()`, we are returned a `Runner` object, which has the following methods/properties:
- stop(): Promise<void> - stops the runner from accepting new jobs, and returns a promise that resolves when all the in progress tasks (if any) are complete.
- addJob: AddJobFunction - see addJob.
- promise: Promise<void> - a promise that resolves once the runner has completed.
- events: WorkerEvents - a Node.js EventEmitter that exposes certain events within the runner (see WorkerEvents).

ex. we can add a job to the queue in response to another job being added to the queue:
```js
await runner.addJob("testTask", {
  thisIsThePayload: true,
});
```

### Events
We can listen to events like so:
```js
runner.events.on("job:success", ({ worker, job }) => {
  console.log(`Hooray! Worker ${worker.workerId} completed job ${job.id}`);
});
```

To realize the importance and place of Workers, we have to break asynchronous tasks down into 2 types:
- async tasks, where the process of execution is dependent on the response to continue on to execute the next lines of code.
  - ex. When the Express server is making database queries, the it has to wait for the data to return before it can continue on with its operation. This is why we use `async`/`await`, because we need that data
- async tasks, where the 2 processes are realistically distinct from one another, and can be completely decoupled.
  - ex. When we want to send registration success emails to users, we don't want to tie up the Express server thread doing this task. It is something that can be handed off to some other service to handle for us.
