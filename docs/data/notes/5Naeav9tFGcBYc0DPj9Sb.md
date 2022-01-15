
Batching is the process of taking a group of requests, combining them into one, and making a single request with the same data that all of the other queries would have made. 
- This is usually done with a timing threshold. For example, with a threshold of 50ms, if a query is made from a component, instead of making the query immediately, the client waits 50ms. If any other queries are requested in that 50ms, all of those additional queries are requested at the same time, rather than separately.

Naturally, batched operations are always as slow as the slowest operation in the batch.
- The downside of this comes if one of the underlying services is slower than the rest, the effects may be felt across the whole client app.

Additionally, batching makes it much harder to debug network traffic.

Because of its drawbacks, it's not recommended to batch unless there are performance issues that can't be resolved through some other means (like CDN caching)

### In Graphql
In GraphQL apps, batching usually takes one of two forms. 
1. take all operations and combine them into a single operation using the `alias` feature in GraphQL. 
    - This approach is not recommended, however, since this removes the ease of tracking metrics on a per-operation basis and adds additional complexity to the client.

2. send an array of operations to a GraphQL server, having the server recognize the request as an array of operations instead of a single one, and handle each operation separately. 
    - This method still requires only a single round-trip, while retaining the ability to track single operation performance. Apollo Client handles batching like this using `apollo-link-batch-http`. It’s also worth noting that this method requires the server to be able to receive an array of operations as well as single operations. Luckily, `apollo-server` supports this out of the box.
