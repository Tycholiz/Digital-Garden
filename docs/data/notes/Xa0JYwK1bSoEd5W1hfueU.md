
Domain events can be used to explicitly implement side effects of changes within your domain
- in other words, we can use domain events to explicitly implement side effects across multiple aggregates.

An event is something that has happened in the past.

A domain event is something that happened in the domain that you want other parts of the same domain (in-process) to be aware of.
- The notified parts usually then react somehow to the events.

ex. The system event might be the actual psi reading of the tire pressure, but the domain event would be "low tire pressure". In this scenario, the service that is consuming this domain event cares only when the tire pressure is low, they don't care if everything else is fine.

An important benefit of domain events is that side effects can be expressed explicitly.

Domain events are similar to messaging-style events, with one important difference:
- with regular messaging, a message is always sent asynchronously and communicated across processes and machines, which is useful for integrating multiple [[Bounded Contexts|general.principles.DDD.bounded-context]], microservices, or even different applications.
- with domain events, you want to raise an event from the domain operation you are currently running, but you want any side effects to occur within the same domain.

The domain events and their side effects (the actions triggered afterwards that are managed by event handlers) should occur almost immediately, usually in-process, and within the same domain. Thus, domain events could be synchronous or asynchronous. Integration events, however, should always be asynchronous.

Domain events is a preferred way to trigger side effects across multiple aggregates within the same domain

It's important to ensure that, just like a database transaction, either all the operations related to a domain event finish successfully or none of them do.

### Rationale
Without domain events, we would just code the functionality we need close to what triggers the event. As a result, this business-logic gets coupled, implicitly, to the code. This means we'd have to look into that code itself to realize that the rule is implemented there.

On the other hand, if we use Domain Events, we make the business logic explicit.

In short, domain events help you to express, explicitly, the domain rules, based in the ubiquitous language provided by the domain experts.

### Example
Imagine we had a main event bus for our entire system for tracking vehicle events, such as geofence crossing, tire pressure, low battery etc.

Within our specific domain, it wouldn't make much sense to subscribe directly to these events because _________

Instead, we define domain events, which are events for what we really care about (example here?)
