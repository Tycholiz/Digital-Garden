
In GraphQL apps, batching usually takes one of two forms. 
1. take all operations and combine them into a single operation using the `alias` feature in GraphQL. 
    - This approach is not recommended, however, since this removes the ease of tracking metrics on a per-operation basis and adds additional complexity to the client.

2. send an array of operations to a GraphQL server, having the server recognize the request as an array of operations instead of a single one, and handle each operation separately. 
    - This method still requires only a single round-trip, while retaining the ability to track single operation performance. Apollo Client handles batching like this using `apollo-link-batch-http`. It’s also worth noting that this method requires the server to be able to receive an array of operations as well as single operations. Luckily, `apollo-server` supports this out of the box.
