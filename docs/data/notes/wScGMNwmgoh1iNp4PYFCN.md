
An aggregate is a pattern in DDD whereby we treat a cluster of domain objects as a single unit.
- ex. an order and its line-items, these will be separate objects, but it's useful to treat the order (together with its line items) as a single aggregate.

If executing a command related to one aggregate instance requires additional domain rules to be run on one or more additional aggregates, you should design and implement those side effects to be triggered by domain events.
- ex. When the user initiates an order, the Order Aggregate sends an OrderStarted domain event. The OrderStarted domain event is handled by the Buyer Aggregate to create a Buyer object in the ordering microservice, based on the original user info from the identity microservice (with information provided in the CreateOrder command).

As shown below, a domain event should be used to propagate state changes across multiple aggregates within the same domain model.
- this shows how consistency between aggregates is achieved by domain events.

![](/assets/images/2021-11-26-10-37-44.png)

An aggregate will have one of its component objects be the aggregate root, which acts as the interface to the outside world.
- The root can thus ensure the integrity of the aggregate as a whole.

Aggregates are the basic element of transfer of data storage - you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.
