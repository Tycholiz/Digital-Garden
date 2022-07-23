
Batching is the process of taking a group of requests, combining them into one, and making a single request with the same data that all of the other queries would have made. 
- This is usually done with a timing threshold. For example, with a threshold of 50ms, if a query is made from a component, instead of making the query immediately, the client waits 50ms. If any other queries are requested in that 50ms, all of those additional queries are requested at the same time, rather than separately.

In the batch processing world, the inputs and outputs of a job are files (perhaps on a distributed filesystem).

Naturally, batched operations are always as slow as the slowest operation in the batch.
- The downside of this comes if one of the underlying services is slower than the rest, the effects may be felt across the whole client app.

Additionally, batching makes it much harder to debug network traffic.

Because of its drawbacks, it's not recommended to batch unless there are performance issues that can't be resolved through some other means (like [[deploy.distributed.CDN]] caching)
