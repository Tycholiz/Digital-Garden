
A replica set is a group of `mongod` processes that maintain the same dataset.

Replica sets provide redundancy and high availability.

A replica set contains several data bearing nodes and optionally one arbiter node. 

### Primary and Secondary nodes
Of the data bearing nodes, one and only one member is deemed the primary node, while the other nodes are deemed secondary nodes.
- The primary node receives all write operations and records all changes to its data sets in its operation log (ie. `oplog`).
- The secondaries replicate the primary's oplog and apply the operations to their data sets (asynchronously) so they are brought in sync with the primary node.
- If the primary is unavailable, an eligible secondary will hold an election to elect itself the new primary. 

### Arbiter
In some circumstances (such as you have a primary and a secondary but cost constraints prohibit adding another secondary), you may choose to add a 
mongod instance to a replica set as an arbiter. An arbiter participates in elections but does not hold data

### Failover
When a primary does not communicate with the other members of the set for more than the configured `electionTimeoutMillis` period (10 seconds by default), an eligible secondary calls for an election to nominate itself as the new primary. The cluster attempts to complete the election of a new primary and resume normal operations.
- until the election is complete, the replica set cannot process write operations.
- The median time before a cluster elects a new primary should not typically exceed 12 seconds