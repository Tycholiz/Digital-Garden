
EventBridge is a serverless [[event bus|general.arch.SOA.bus]], implementing the [[pub/sub|general.patterns.messaging.pubsub]] architectural pattern.
- goal is to make it easier to build event-driven apps at scale using events originating from our applications.

EventBridge logically separates routing using event buses, and you implement the routing logic using rules (ie. it has a built-in rules engine).
- You can filter and transform incoming messages at the service level, and route events to multiple targets
	- Matched events can be sent to a host of target destination, such as [[Lambdas|aws.svc.lambda]], other Event Bridges, or even HTTP endpoints.
- It also comes with schema registry

EventBridge may be used to help us implement an [[Event-driven architecture|general.arch.event]]

Data Sources may include:
- our own applications
- SaaS applications ([list of 3rd party integrations](https://aws.amazon.com/eventbridge/integrations/))
	- meaning we can authorize supported SaaS providers to send events directly from their EventBridge event bus to partner event buses in your account.
- AWS services

Targets may include:
- HTTP endpoints
- other AWS services (e.g. other event buses, [[Lambdas|aws.svc.lambda]], [[Cloudwatch|aws.svc.cloudwatch]])

A single bus of EventBridge can handle tens of thousands of concurrent events, so it is conceivable that you'd only have a single bus for one business case.
- In other words, each user of your application wouldn't need have their own associated bus; all events no matter which users they are associated with can exist within a single bus.

EventBridge provides at-least-once event delivery to targets, including retry with exponential backoff for up to 24 hours.

### How it works
EventBridge receives an event and applies a rule to route the event to a target.
- Each event contains a json event body, as well as several metadata fields.

We the programmer provide EventBridge with a list of rules, which it uses to match events to targets
- These matches are made based on either [event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html), or schedules.
	- ex. when an [[EC2|aws.svc.EC2]] instance changes from pending to running, you can have a rule that sends the event to a Lambda function.

All events that arrive at the doorstep of EventBridge will be associated with a particular [event bus](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-bus.html)
- rules are tied to one specific event bus.
- EventBridge comes with a default event bus out of the box, which receives events from AWS services.

Event bridge doesn't enforce ordering.
- if ordering is something we need, [[kafka]] might be a more suitable alternative

* * *

### Targets
Targets allow us to do things like invoke a Lambda, start execution on a [[step function|aws.svc.step-functions]], start a task with [[ESC|aws.svc.ECS]] or [[Fargate|aws.svc.fargate]]

The event bridge can be provisioned and manipulated programmatically to route incoming events to various destinations.

The [[rules engine|general.arch.SOA.rules-engine]] allows for advanced pattern matching based on source, subject, version, or any key of the JSON body. Matched events can then be sent to a host of target destinations including Lambdas, other EventBridges, or even HTTP endpoints.

### Rules
The rules engine provides the business logic of our EventBridge. This is where we (the configurer of EventBridge) can create rules that match selected events in the stream and route them to targets to take action.

This is where we the configurer of EventBridge determine which events we are interested in, and what we will do when we encounter those events.
- namely, the event is routed to the target(s) associated with that rule.

You can also use rules to take action on a predetermined schedule. For example, you can configure rules to:
- Automatically invoke an Lambda function to update DNS entries when an event notifies you that Amazon EC2 instance enters the running state.
- Direct specific API records from CloudTrail to an Amazon Kinesis data stream for detailed analysis of potential security or availability risks.
- Periodically invoke a built-in target to create a snapshot of an Amazon EBS volume.


### Schema Registry
EventBridge has a built-in [schema registry](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-schema.html)

### Competing Solutions
- Google PubSub
