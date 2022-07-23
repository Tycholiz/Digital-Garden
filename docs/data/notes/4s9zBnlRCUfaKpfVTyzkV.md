
Subscriptions offer real-time connection from the client to the server that allows the client to get immediately informed about events happening server-side
- when a client subscribes to an event, it will hold a steady connection to the server. when this event happens, the server will push that corresponding data to the client
Therefore, subscriptions are event-based, acting in response to what just happened.
- We can see this in the subscription nomenclature: `commentAdded`, `paymentMethodAdded`
- Unlike queries and mutations that follow a typical “request-response-cycle”, subscriptions represent a *stream* of data sent over to the client.

You should use subscriptions for the following:
- Small, incremental changes to large objects.
  - Repeatedly polling for a large object is expensive, especially when most of the object's fields rarely change. Instead, you can fetch the object's initial state with a query, and your server can proactively push updates to individual fields as they occur.
- Low-latency, real-time updates.
  - For example, a chat application's client wants to receive new messages as soon as they're available.

```gql
subscription {
  newPerson {
    name
    age
  }
}
```
Whenever a newer mutation is performed that creates a new `Person`, the server sends the information about this person to the client
