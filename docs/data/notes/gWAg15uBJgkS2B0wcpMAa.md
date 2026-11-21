
The name Redis means *Remote Dictionary Server*

With Redis, the data is (ideally) all in memory, making the lookups much faster.
- anything slower than 10ms is considered slow, and is indicative of an issue.

Redis is a good fit in situations where you want to share some transient, approximate, fast-changing data between servers, and where it’s not a big deal if you occasionally lose that data for whatever reason
ex. a good use case is maintaining request counters per IP address (for rate limiting purposes) and sets of distinct IP addresses per user ID (for abuse detection).
ex. Real time Analytics - you can use Redis to store user identities and their transaction details when implementing a real-time fraud detection service.
ex. You can use Redis to queue application (FIFO) tasks that take a long time to complete

- used as a database, cache, message broker, and message queue.
- All Redis data resides in-memory RAM
- Redis delivers response times of less than 1ms, enabling millions of requests per second
- Redis is a popular choice for caching, session management, gaming, leaderboards, real-time analytics, geospatial, ride-hailing, chat/messaging, media streaming, and pub/sub apps.
- Redis processes commands using a single thread (i.e. while a command is being executed all others need to await until executing one ends).

You can run atomic operations on the contents of the redis cache.

Redis supports [asynchronous replication](https://redis.io/topics/replication)

By default, Redis writes data to a file system at least every 2 seconds, meaning in event of system failure, only the last few seconds of data would be lost.

### Data storage and access
Generally, data in your cache will be stored in a highly [[normalized|db.design.normalization]] way, often only one layer deep.

When desigining your keys, think chiefly about how that data will be accessed. Consider the access pattern that your app uses to its main database. What type of data are you querying on? For instance, if your app's business logic dictates an important access pattern as `albumsByArtist`, then our cache key structure should follow a similar pattern. Put another way, we want to be able to access our cache data with a similar mindset to how we are accessing our main database's data.

### Cache invalidation
One of the ways to achieve cache invalidation in Redis is through a built-in Pub/Sub system. It can be usd to send invalidation messages to clients listening, so that the client knows it should fetch again if it wants up to date data.
- This can be made to work but is tricky and costly from the point of view of the bandwidth used, because often such patterns involve sending the invalidation messages to every client in the application, even if certain clients may not have any copy of the invalidated data

## Client-side Caching (called Tracking)
There are 2 modes to client-side caching in Redis
- *default* - server remembers which clients it sends data to. When the data is modified, it sends an invalidation message to the client, letting it know that its data is no longer up to date. This costs memory in the server side, since the server must keep the list of clients in memory.
- *broadcast* - server does not attempt to remember what keys a given client accessed. Instead clients subscribe to key prefixes such as `object:` or `user:,` and will receive a notification message every time a key matching such prefix is touched.
    - does not consume any memory on the server side, but instead sends more invalidation messages to clients.
    - The server does not store anything in the invalidation table. Instead it only uses a different Prefixes Table, where each prefix is associated to a list of clients.

This is an example of the protocol:
```
Client 1 -> Server: CLIENT TRACKING ON
Client 1 -> Server: GET foo
(The server remembers that Client 1 may have the key "foo" cached)
(Client 1 may remember the value of "foo" inside its local memory)
Client 2 -> Server: SET foo SomeOtherValue
Server -> Client 1: INVALIDATE "foo"
```

This looks great superficially, but if you think at 10k connected clients all asking for millions of keys in the story of each long living connection, the server would end up storing too much information. For this reason Redis uses two key ideas in order to limit the amount of memory used server side, and the CPU cost of handling the data structures implementing the feature:
- We must create an *invalidation table* which stores rows of clients that have cached a given key.
    - Such invalidation table can contain a maximum number of entries, if a new key is inserted, the server may evict an older entry by pretending that such key was modified. Once this happens, it sends an invalidation message to the clients.

clients do not need, by default, to tell the server what keys they are caching. Every key that is mentioned in the context of a read only command is tracked by the server, because it could be cached.

This has the obvious advantage of not requiring the client to tell the server what it is caching. Moreover in many clients implementations, this is what you want, because a good solution could be to just cache everything that is not already cached, using a first-in first-out approach: we may want to cache a fixed number of objects, every new data we retrieve, we could cache it, discarding the oldest cached object. More advanced implementations may instead drop the least used object or alike.

## UE Resources
- [Redis Tutorials by Flavio](https://flaviocopes.com/tags/redis/)
- [Realtime delivery Architecture at Twitter](https://www.infoq.com/presentations/Real-Time-Delivery-Twitter/)

## Resources
- [RedisJSON](https://redis.io/docs/stack/json/) - JSON support for Redis, effectively adding the JSON type, and letting us store, update, and retrieve JSON values
