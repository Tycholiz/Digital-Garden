
### Enterprise Service Bus (ESB)
An ESB implements a communication system between services in a Service Oriented Architecture.

An ESB is a pattern, rather than any specific technology. We implement it using event-driven and message-oriented middleware (MOMs) in combination with [[message queues|general.arch.message-queue]] as technology frameworks.

The idea is you have some kind of pipeline with computers connected to it, and whenever one of them sends a message, it’s dispatched to all of the others. Then, they decide if they want to consume the given message or just discard it.

An ESB represents a software architecture for [[distributed computing|deploy.distributed]]
- It is a special variant of the [[client-server architecture|general.arch.client-server]], where any service can act as the client or the server.

ESB promotes agility and flexibility when it comes to high-level protocol communication between services.

The idea of a [[hardware bus|hardware.bus]] is not all that different to an event bus.
- in the analogy, the machines are your hardware components, the message is the event or the data you want to communicate, and the pipeline is your `EventBus` object.

Event buses help loosen coupling between classes and promotes a [[publish-subscribe|general.patterns.messaging.pubsub]] pattern. It also help components interact without being aware of each other.
- Applications can integrate each other in a loosely coupled manner through this mediator (ie. the bus), so that they will not be dependent on each other's interfaces.

A bus uses the [[pub-sub|general.patterns.messaging.pubsub]] model.
