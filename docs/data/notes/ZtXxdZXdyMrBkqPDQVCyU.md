
The idea with the observer pattern is that an object (the `subject`) maintains a list of all its dependents. Whenever a piece of state in the subject changes, all of the observers are automatically notified. This is normally accomplished by the subject calling the observer's `update()` method.
- The sole responsibility of a *subject* is to maintain a list of observers and to notify them of state changes by calling their update() operation. 
- The responsibility of *observers* is to register (and unregister) themselves on a subject (to get notified of state changes) and to update their state (synchronize their state with the subject's state) when they are notified. This makes subject and observers loosely coupled.
    - Observers can be added and removed independently at run-time. This is very similar to [[pub-sub|general.architecture.pubsub]]

Observer pattern is used in event handling systems that are generally distributed. In this case, the subject is usually a "stream of events", while the observers are "sinks of events". The observers themselves are physically separated and have no control over their emitted events from the stream-source (ie. subject).

The observer pattern is well-suited to a process where data arrives from some input that is not available to the CPU at startup, but instead arrives "at random" (HTTP requests, GPIO data, user input from keyboard/mouse/..., distributed databases and blockchains, ...).

The Observer pattern addresses the following problems:

- A one-to-many dependency between objects should be defined without making the objects tightly coupled.
- It should be ensured that when one object changes state, an open-ended number of dependent objects are updated automatically.
- It should be possible that one object can notify an open-ended number of other objects.