
## What is it?
A system with lots of objects can lead to complexities when a client wants to subscribe to events. The client has to find and register for each object individually, if each object has multiple events then each event requires a separate subscription.

To get around this, we can add a layer in between called an Event Aggregator which does the work of registering to the events of many different objects, combining them, then allowing clients to register with the aggregator itself.

An Event Aggregator is a simple element of [[indirection|general.terms.indirection]]

a Event Aggregator also simplifies the memory management issues in using [[observers|general.patterns.behavioural.observer]].
