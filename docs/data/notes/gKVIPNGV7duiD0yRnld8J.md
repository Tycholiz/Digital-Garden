
## What is it?
Replication means keeping a copy of the same data on multiple machines that are connected via a network.

Each node that stores a copy of the database is called a replica. 

If you look at two database nodes at the same moment in time, you’re likely to see different data on the two nodes, because write requests arrive on different nodes at different times (replication lag).
- These inconsistencies occur no matter what [[replication strategy|db.distributed.replication.strategies]] the database uses (single-leader, multi-leader, or leaderless replication).
- Most replicated databases provide at least eventual consistency
	- in other words, this means that the inconsistency among replicas is temporary and eventually resolves itself.

## Why do we do it?
- Latency: To keep data geographically close to your users (and thus reduce latency)
- Availabrlity: to allow the system to continue working even if some nodes have failed
- Scalability: to [[scale out|deploy.scaling#horizontal-scaling-aka-scaling-out]] the number of machines that can serve read queries (and thus increase read throughput)
- Network fault-tolerance: to keep the application working when there is a network interruption

In a world where our data didn't change over time, replication would be simple: we would just copy the data to each node and be done.
- what makes replication hard is figuring out how to handle changes to replicated data.

Normally, replication is quite fast: most database systems apply changes to followers in less than a second.
- However, there are circumstances when followers might fall behind the leader by several minutes or more; for example:
	- if a follower is recovering from a failure
	- if the system is operating near maximum capacity
	- if there are network problems between the nodes

There are tradeoffs to consider when implementing replication (both are often configuration options in databases):
- synchronous or asynchronous replication
- how to handle failed replicas

## Replication strategies
[[db.distributed.replication.strategies]]