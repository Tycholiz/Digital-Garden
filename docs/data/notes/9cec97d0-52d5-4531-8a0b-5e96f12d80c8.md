
## Router
- A router is essentially just a container for a set of middleware, grouped by the fact they all have to do with http methods and routes
- The router is an isolated (meaning it operates independently of other routers) instance of middleware and routes. Therefore it can only perform middleware and routing functions.
- The router can be thought of as a mini-application
- A route is a combination of a path and a callback (called the route-handler)
