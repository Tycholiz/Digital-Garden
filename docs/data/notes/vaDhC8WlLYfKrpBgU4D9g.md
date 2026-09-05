
CQRS is an architectural pattern that separates reading and writing into two different models. This means that every method should either be a Command that performs an action or a Query that returns data. A Command cannot return data and a Query cannot change the data
- ex. While not CQRS in principle, Redux attempts to follow the spirit of this pattern. Consider that to write to state we must dispatch an action, and to read state we must use selectors. The implementations of reading and writing to state are decoupled.

A common implementation of CQRS occurs in databases with Leader-Follower [[replication|db.distributed.replication]], whereby the read requests are routed to the follower database instances, while the write requests are routed to the leader database instance.

The CQRS pattern offers high performance and high availability at the cost of stale reads (assuming replication is done asynchronously)

In CQRS, we use different interfaces to read and update data. This is as opposed to CRUD (Create, Read, Update, Delete), where we use a single interface to perform all the reads and updates.

CQRS is implemented using [[general.patterns.event-sourcing]]

While implementing a CQRS system, all the commands are modelled as events. So whenever a user performs any action, a command is dispatched as an event and the event is stored in a store.
- Whenever we want to read data, we take the data at its initial state and apply all the commands to the data to get the most up to date data.

Storing data is normally quite straightforward if you don’t have to worry about how it is going to be queried and accessed; many of the complexities of schema design, indexing, and storage engines are the result of wanting to support certain query and access patterns. For this reason, you gain a lot of flexibility by separating the form in which data is written from the form it is read, and by allowing several different read views. This is the basis for the idea of CQRS.
- The traditional approach to database and schema design is based on the fallacy that data must be written in the same form as it will be queried.
- Debates about [[normalization and denormalization|db.design.normalization]] become largely irrelevant if you can translate data from a write-optimized event log to read-optimized application state: it is entirely reasonable to denormalize data in the read-optimized views, as the translation process gives you a mechanism for keeping it consistent with the event log.

Conceptually, you always implement CQRS by using two different models, one for updates and one for queries.
- You populate a model specifically designed to make updates easy from commands, then serialize it, and you populate a distinctly designed model, designed to make the life of querying clients easy, from data store queries, and the put it on the wire and send it to clients. How you populate each of the two distinct models, whether you use a database view, an in-memory cache, aggregate SQL queries or some other mechanism is irrelevant. As long as you stick to the two distinct models pattern it's CQRS.

You don't necessarily implement CQRS at the database level, or at the database query level.


# UE Resources
- https://martinfowler.com/bliki/CQRS.html
- https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs
