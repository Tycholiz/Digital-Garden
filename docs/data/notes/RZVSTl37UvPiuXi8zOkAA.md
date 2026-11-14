
`setTimeout` allows us to run a function *once* after the interval of time.

setTimeout says "call *this* function (arg1) after *this many* seconds (arg2)

Both `setTimeout` and `setInterval` allow us to execute some code at a later point in time (ie. "schedule a call")

You can pass arguments to the function you pass to `setTimeout` as the 3rd+ parameter(s)
- ex. `setTimeout(executePoll, this.pollIntervalMs, resolve, reject)`. Here, `resolve` and `reject` are parameters of the `executePoll` method.