
A system that allows us to query a database tells us the current state of everything. A system that allows Event Sourcing also tells us how we got here. In other words, the system keeps records of all changes that have happened.
- Event Sourcing ensures that all changes to application state are stored as a sequence of events. Not just can we query these events, we can also use the event log to reconstruct past states, and as a foundation to automatically adjust the state to cope with retroactive changes.

every change to the state of an application is captured in an event object, and that these event objects are themselves stored in the sequence they were applied for the same lifetime as the application state itself.
- To be clear, we are persisting two different things: an application state (ex. DB) and an event log.

The key to Event Sourcing is that we guarantee that all changes to the domain objects (ex. what is stored in the DB) are initiated by the event objects
- This shows that when we want to change something in our database, our event system is first updated with the new log. Only once that log has been entered can our database be updated as well.
	- An implication of this is that we could dump the whole application state, and rebuild it from the log.
	- Another implication is that we can see the state of the application as a timeline, analogous to Git branching.

A common example of an application that uses Event Sourcing is a version control system

[[CQRS|general.patterns.architectural.CQRS]] and event sourcing often go hand-in-hand

# UE Resources
- https://martinfowler.com/eaaDev/EventSourcing.html
- https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing
- [EventStoreDB](https://www.eventstore.com/eventstoredb)
