
## Atomic Broadcast
An atomic broadcast is a broadcast sent out to all nodes of a distributed system.

The purpose of an atomic broadcast is to address the challenges of distributed consensus, [[fault tolerance|deploy.distributed.fault-tolerance]], and maintaining data consistency in a distributed system. The goal is to achieve consensus among distributed nodes and maintain strong consistency across the system.
- It does this by ensuring that a set of messages or events are delivered to all participants (nodes) in a distributed system in the same order and in an atomic manner, allowing for coordinated and consistent actions across the entire system

There are 2 properties of an atomic broadcast:
1. All nodes in the system receive the messages in the exact same order. This guarantees that the events are processed in the same sequence, ensuring consistency across the distributed system.
2. The broadcast operation is atomic, meaning either all nodes receive all messages in the agreed-upon order, or none of them do. There is no partial or inconsistent delivery.

When implemented correctly, an atomic broadcast will have the following properties:
- Validity: if a correct participant broadcasts a message, then all correct participants will eventually receive it.
- Uniform Agreement: if one correct participant receives a message, then all correct participants will eventually receive that message.
- Uniform Integrity: a message is received by each participant at most once, and only if it was previously broadcast.
- Uniform Total Order: the messages are totally ordered in the mathematical sense; that is, if any correct participant receives message 1 first and message 2 second, then every other correct participant must receive message 1 before message 2.

[[zookeeper]] uses an atomic broadcast as its basic building block

### Use cases
- Fault Tolerance and Replication: Atomic broadcast helps maintain fault tolerance in the event of node failures. Even if some nodes crash and recover, the atomic broadcast ensures that they receive the missed messages in the same order as other nodes, preventing inconsistencies.
- Data Replication: In distributed databases or storage systems, atomic broadcast is used to replicate data changes across multiple replicas to maintain strong consistency.
    - ex. in a distributed database, atomic broadcasts may occur every time there is a write operation to maintain data consistency across all replicas.
    - ex. in a Slack application, the atomic broadcast should achieve the following:
        - validity: all users will eventually receive the message published by a user in a Slack channel
        - integrity: a published chat message is received by each user not more than once
        - total order: all users receive the chat messages in the identical order
- Distributed Consensus: Paxos (a consensus protocol) uses Atomic Broadcasts to ensure that all nodes agree on a single shared state or decision. 
    - here, the atomic broadcast ensures that all nodes have the same view of the system's state.
- Replicated State Machines: In replicated state machine systems, where multiple nodes maintain identical copies of a state machine, atomic broadcast is used to ensure that all state machines receive the same sequence of commands in the same order.

### Example: Slack
When a user sends a message in a Slack channel, how can we make guarantees about the integrity of the received messages? How can we be sure that each client received all messages and in the proper order?

1. User A sends a message in the Slack channel.
2. The Slack backend server receives the message from User A and needs to ensure that this message is delivered to all other members of the channel.
3. To achieve atomic broadcast, Slack can utilize a consensus algorithm, such as Paxos or Raft, to ensure that the message is consistently delivered and processed in the same order by all replicas of the channel across multiple servers.
4. The consensus algorithm allows the servers hosting the channel to agree on the order of the messages to be delivered to the users. It ensures that all servers have the same set of messages in the same order, avoiding any discrepancies.
5. Once consensus is reached, the message is replicated and delivered to all other members of the channel. This ensures that all users see the same message in the same sequence, maintaining consistency across the system.
6. Slack can implement a mechanism to acknowledge message delivery to the sender and other members of the channel. This way, all users are aware that the message has been successfully broadcasted and received.