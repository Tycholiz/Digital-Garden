
The goal of a hexagonal architecture is to have a loose coupling between components of a system. To connect these systems, we use thin layers which are manisfested as [[adapters|general.patterns.structural.adapter]] and ports.
- the port uses a particular protocol, and define an abstract API.
  - ex. ports for event sources (e.g. UIs), ports for notifications, ports for database etc.
- the adapters serve as glue between the components (via the ports) and the outside world.
  - there may be several adapters for a single port. For instance, if data could be provided through various means, such via a GUI, via an automated data source, via a test script... This would necessitate multiple adapters for the single port.

The idea of Hexagonal Architecture is to put inputs and outputs at the edges of our design. 
- Business logic should not depend on whether we expose a REST or a GraphQL API, and it should not depend on where we get data from — a database, a microservice API exposed via gRPC or REST, or just a simple CSV file.
- The pattern allows us to isolate the core logic of our application from outside concerns, meaning you can easily change data source details without a significant impact or major code rewrites to the codebase.

With a traditional layered architecture, we would have all of our dependencies point in one direction, each layer above depending on the layer below. The transport layer would depend on the interactors, the interactors would depend on the persistence layer.
In Hexagonal Architecture all dependencies point inward — our core business logic does not know anything about the transport layer or the data sources. Still, the transport layer knows how to use interactors, and the data sources know how to conform to the repository interface.

The purpose is to have a highly modular system that allows us to swap out components (e.g. the database, the UI etc.), and only be required to make changes to the adapter.
- an added benefit is that we greatly simplify the testing strategy.

The hexagonal architecture can be thought of as the precursor to the [[microservice|general.arch.microservice]] architecture.

## Breaking it down
![](/assets/images/2022-04-26-14-28-26.png)

### Inner components
The inner components do not reach into the outside world. They don't know how to talk to the database, nor the web. In other words, they have no technology concerns.

A good practice is to use the principle of Dependency Inversion, with the internal rule that only outer ring components may depend on inner ring components, and never the contrary.

### Outer components
The outer components each serve a distinct purpose.
- ex. we can have a User Component which knows how to pass data to and from Users. The User component is connected to the Inner components via the port, and in this case we might decide we need 2 adapters: an HTTP adapter and a GUI adapter.