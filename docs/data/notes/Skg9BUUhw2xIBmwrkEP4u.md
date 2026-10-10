
A messaging pattern describes how two different parts of an application (or different systems) connect and communicate with each other.
- In actuality, protocols like REST are indeed messaging protocols, but we don't think of them as such. Typically when we talk about messaging patterns, we are referring to situations where we have this third entity between the provider/consumer of the data.

message-passing communication is usually one-way: a sender normally doesn't expect to receive a reply to its messages. It is possible for a process to send a response, but this would usually be done on a separate channel.

a message is just a sequence of bytes with some metadata, so you can use any encoding format

Two types of messaging patterns are available − 
1. point to point 
	- a particular message can be consumed by a maximum of one consumer only, and it disappears from the queue once a consumer reads it
	- The typical example of this system is an Order Processing System, where each order will be processed by one Order Processor, but Multiple Order Processors can work as well at the same time.
2. [[Pub-Sub|general.patterns.messaging.pubsub]] 
	- Most of the messaging patterns follow pub-sub.

# Overview
Imagine we have 2 services: OrderService and InvoiceService. In order to pass data from OrderService downstream to InvoiceService, we set up a message queue in the OrderService. Then, InvoiceService would read from it.

If we need to add more downstream services, then we need to modify the upstream service (ie. OrderService) to instruct it to write a message queue. This creates more complex high-level service relationships that are hard to maintain, since future downstream services now become reliant on that upstream service to update its message queues.
- This burden is what causes people to go for more of a [[bus|general.arch.SOA.bus]]-based solution

In a typical message-queueing implementation, a developer installs and configures message-queueing software (a queue manager or broker), and defines a named message queue. Or they register with a message queuing service.
- An application then registers a software routine that "listens" for messages placed onto the queue.
- Second and subsequent applications may connect to the queue and transfer a message onto it.
- The queue-manager software stores the messages until a receiving application connects and then calls the registered software routine. The receiving application then processes the message in an appropriate manner.

Some patterns for implementing messages are:
- Message Queues
	- ex. [[SQS|aws.svc.SQS]]
- [[Pub/Sub|general.patterns.messaging.pubsub]]
	- ex. [[SNS|aws.svc.SNS]]
- Event buses
	- ex. [[EventBridge|aws.svc.event-bridge]]

## Fan out
Fan-out is a messaging pattern where messages are broadcast in parallel in a one-to-many arrangement.
- this pattern can be seen in the [[pub/sub|general.patterns.messaging.pubsub]] messaging system, since a single sender can send messages to many receivers.

![](/assets/images/2022-04-07-21-41-28.png)

## Message Queue
Message queues provide some guarantee of reliability, in that the number of incoming messages equals the number of outgoing messages

## Message Broker
A message broker is an intermediary computer program module that translates a message from the formal messaging protocol of the sender to the formal messaging protocol of the receiver.

Message brokers are used as follows: one process sends a message to a named queue or topic, and the broker ensures that the message is delivered to one or more consumers of or subscribers to that queue or topic. 
- There can be many producers and many consumers on the same topic.

### Examples
- [[kafka]], though more of an event-streaming platform
- RabbitMQ
- AWS Kinesis
- Google Cloud Pub/Sub
- Redis (not a message broker per se, but rather has messaging brokering as one of its capabilities)
- NATS
