
An adapter is a wrapper that converts the interface of an existing class into another interface that would be expected by a different client. It is a way of bridging the gap between incompatible interfaces.
- It is often used to make existing classes work with others without modifying their source code.
- ex. the [[browser]] uses an adapter to convert the interface of a [[browser.DOM]] of an XML document into a tree that can be displayed.

Anal: we have adapters for electrical sockets when visiting foreign countries. These bridge the gap between 2 interfaces that are incompatible with one another 

An adapter may also be known as a *wrapper*, though that name may also refer to a [[decorator|general.patterns.structural.decorators]].

An adapter allows two incompatible interfaces to work together. It does this by converting the interface of one class into an interface expected by the clients.
- Interfaces may be incompatible, but the inner functionality should suit the need. 

The adapter design pattern solves problems like:
- How can a class be reused that does not have an interface that a client requires?
- How can classes that have incompatible interfaces work together?
- How can an alternative interface be provided for a class?

Often an (already existing) class can't be reused only because its interface doesn't conform to the interface clients require. The adapter design pattern describes how to solve such problems:
- Define a separate adapter class that converts the (incompatible) interface of a class (adaptee) into another interface (target) clients require.
- Work through an adapter to work with (reuse) classes that do not have the required interface.

The key idea in this pattern is to work through a separate adapter that adapts the interface of an (already existing) class without changing it.

