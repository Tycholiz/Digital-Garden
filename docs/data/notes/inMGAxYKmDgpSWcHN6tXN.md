
Middleware is a stack of functions that gets called in a chain. However, the next function in that chain does not need to get called necessarily. The middleware that is currently handling the request decides if the request will complete right then and there, continue on to the next middleware in the stack with `next()`, or die instantly.
- middlewares are the key to flexibility and modularity in Express
- A middleware is a function that occurs in the lifecycle of a http request before the request hits the server, or before the response gets to the client
- These functions have access to the request and response objects, and can modify/read however they want
- When we add middleware to express with `app.use()`, we are appending items to `Server.prototype.stack` in Connect. When the server receives a request, it iterates over the stack, calling the `(req, res, next)` method.
- Recall that each middleware item will either modify the request object, modify the response object, or call `next()` so the next middleware in the stack is called (?)
- If a middleware does *not* modify the body or the header of the response, it should call `next()`. If it does, it should not call `next()`
- If the current mw function does not end the request-response cycle, it must call `next()` so the next mw in the stack can take over.

Technically, Express itself is just middleware that occurs before the node.js server

middleware by its nature is dumb. It doesn't know which handler will be executed after calling the `next()` function.

Middleware functions can perform the following tasks:
- execute any code.
- make changes to the request and the response objects.
- end the request-response cycle.
- call the next middleware function in the stack.
- if the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.

## Types of middleware
### Application level
using `app.use()`, we can bind a piece of application-level middleware to the app object
- `app.use` will be called each time a request is sent to the server

### Router level
works the same as application-level, except the middleware is bound to an instance of `express.Router()` instead of `express()`

### Error handling

### Built-in
ex. `express.static`, `express.json`
