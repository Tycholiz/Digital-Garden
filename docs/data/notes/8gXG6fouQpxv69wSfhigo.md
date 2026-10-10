
A gateway is an object that encapsulates access to an external system or resource. It is therefore an abstraction of the APIs that our application accesses. Thus, all the benefits that an abstraction typically provides apply here.
- A gateway is typically a simple wrapper. We look at what our code needs to do with the external system and construct an interface that supports that clearly and directly.

![](/assets/images/2021-12-14-14-25-34.png)

The gateway acts on terms that our system uses. It allows access to data sources, which may be queried in varying ways. The gateway takes all of these odd endpoints and ties them up into a nice little bow that can provide a unified interface that is ready to be queried.

Gateways should only include logic that supports this translation between domestic and foreign concepts. Any logic that builds on that should be in clients of the gateway.

The notion of a gateway fits well with that of the [[Bounded Contexts|general.principles.DDD.bounded-context]]
- use a gateway when dealing with something in a different context. The gateway handles the translation between the foreign context and your own.
	- The gateway is a way to implement an Anticorruption Layer

### The problem
Usually we depend on third party APIs to build out our applications. These interfaces often seem awkward from the context of our software
- The API may use different types, require strange arguments, combine fields in ways that don't make sense in our context. Dealing with such an API can result in jarring mismatches whenever its used.

Use a gateway whenever there is a need to access some external software and there is any awkwardness in that external element.
- Rather than let the awkwardness spread through the code, contain it to a single place in the gateway.

The question we have to ask ourselves when considering whether or not to make a gateway is "do we wish to isolate ourselves from the underlying platform?"
- In many cases the platform's facilities are so pervasive that it's not worth going through the effort of wrapping it in a gateway.

### Pros
- Insulates the clients from how the application is partitioned into microservices
- Insulates the clients from the problem of determining the locations of service instances
- Provides the optimal API for each client
- Reduces the number of requests/roundtrips. For example, the API gateway enables clients to retrieve data from multiple services with a single round-trip. Fewer requests also means less overhead and improves the user experience. An API gateway is essential for mobile applications.
- Simplifies the client by moving logic for calling multiple services from the client to API gateway
- Translates from a “standard” public web-friendly API protocol to whatever protocols are used internally
- The API gateway pattern has some drawbacks:

### Cons
- Increased complexity - the API gateway is yet another moving part that must be developed, deployed and managed
- Increased response time due to the additional network hop through the API gateway - however, for most applications the cost of an extra roundtrip is insignificant.


### Connection Object
It's often useful to add a connection object to the basic structure of the gateway. The connection is a simple wrapper around the call to the foreign coded.
- The gateway translates its parameters into the foreign signature, and calls the connection with that signature. The connection then just calls the foreign API and returns its result. The gateway finishes by translating that result to a more digestible form.
- The connection can be useful in two ways.
	1. Firstly it can encapsulate any awkward parts of the call to the foreign code, such as the manipulations needed for a REST API call.
	2. Secondly it acts as a good point for inserting a [[Test Double|testing.test-double]].

The connection object is where we make the HTTP request.

#### Testing
Using a gateway can make a system much easier to test by allowing the test harness to stub out the gateway's connection object.
- particularly important for gateways that access remote services, as it can remove the need for a slow remote call
	- use a gateway here, even if the external API is otherwise okay to use (in which case the gateway would only be the connection object).

* * *

A gateway is usually either an end-point or allows bi-direction communication between dissimilar systems
- In contrast, sinks are just an event input point.

* * *

### API Gateway
API gateway handles requests in one of two ways. 
- Some requests are simply proxied/routed to the appropriate service. 
- Other requests by fanning out to multiple services.

![](/assets/images/2021-12-20-11-28-44.png)

Rather than provide a one-size-fits-all style API, the API gateway can expose a different API for each client
- ex. the Netflix API gateway runs client-specific adapter code that provides each client with an API that’s best suited to its requirements.

#### Backends for Frontends
This is a variation of the API Gateway approach whereby each client gets its own API gateway:
![](/assets/images/2021-12-20-11-30-09.png)

# E Resources
[Martin Fowler article](https://martinfowler.com/articles/gateway-pattern.html)
- includes info on testing with gateway
[Introduce gateway into existing app](https://martinfowler.com/articles/refactoring-external-service.html)
[Simple API Gateway service Java code](https://github.com/cer/event-sourcing-examples/tree/master/java-spring/api-gateway-service)
