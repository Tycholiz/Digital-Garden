
### Enterprise Service Bus (ESB)
An ESB implements a communication system between services in a [[SOA (Service Oriented Architecture)|general.arch.SOA]].

An ESB is a centralized component that performs integrations between applications.
- It performs transformations of data models, handles connectivity, performs message routing, converts communication protocols and potentially manages the composition of multiple requests. 

An ESB is an essential component of an SOA.
- It is possible to implement a SOA without an ESB architecture, but this would be equivalent to just having a bunch of services. Each application owner would need to directly connect to any service it needs and perform the necessary data transformations to meet each of the service interfaces. This is a lot of work (even if the interfaces are reusable) and creates a significant maintenance challenges in the future as each connection is point to point.
- In theory, a centralized ESB offers the potential to standardize—and dramatically simplify—communication, messaging, and integration between services across the enterprise. Hardware and software costs can be shared, provisioning the servers as needed for the combined usage, providing a scalable centralized solution.  A single team of specialists can be tasked (and, if necessary, trained) to develop and maintain the integrations.
- Software applications simply connect (‘talk’) to the ESB and leave it to the ESB to transform the protocols, route the messages, and transform into the data formats as required providing the interoperability to get the transactions execut
    - This enables developers to spend dramatically less time integrating their software with other software.

An ESB is a pattern, rather than any specific technology. We implement it using event-driven and message-oriented middleware (MOMs) in combination with [[message queues|general.patterns.messaging]] as technology frameworks.

The idea is you have some kind of pipeline with computers connected to it, and whenever one of them sends a message, it’s dispatched to all of the others. Then, they decide if they want to consume the given message or just discard it.

An ESB represents a software architecture for [[distributed computing|deploy.distributed]]
- It is a special variant of the [[client-server architecture|general.arch.client-server]], where any service can act as the client or the server.

ESB promotes agility and flexibility when it comes to high-level protocol communication between services.

The idea of a [[hardware bus|hardware.bus]] is not all that different to an event bus.
- in the analogy, the machines are your hardware components, the message is the event or the data you want to communicate, and the pipeline is your `EventBus` object.

Event buses help loosen coupling between classes and promotes a [[publish-subscribe|general.patterns.messaging.pubsub]] pattern. It also help components interact without being aware of each other.
- Applications can integrate each other in a loosely coupled manner through this mediator (ie. the bus), so that they will not be dependent on each other's interfaces.

A bus uses the [[pub-sub|general.patterns.messaging.pubsub]] model.
