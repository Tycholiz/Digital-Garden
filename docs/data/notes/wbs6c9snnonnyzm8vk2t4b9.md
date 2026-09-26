
With multiple replicas, a question inevitably arises: how do we ensure that all the data gets copied to all the replicas? 
- Naturally, every write to the database needs to be processed by every replica. Every replication strategy must ensure that the data is eventually the same in all replicas.

# Replication Strategies
There are three main algorithms for replicating changes between nodes: 
- single-leader 
- multi-leader
- leaderless

## Single-Leader based replication
- a.k.a *active/passive* or *master–slave replication*
- this mode of replication is widespread, and is built into [[postgres|pg]], [[kafka]], [[mongo]] etc.

There are no conflict resolution issues to deal with in single-leader replication.

### Approach
1. A client wants to write to the database. It sends its request to the leader, which writes to its own local storage.
2. When the leader writes to its local storage, it also sends the data change to all of its followers as part of a replication log (or change stream)
3. Each follower applies all of the writes from the replication log
4. Now, the leader and any of the followers can fulfill read requests.

Since we can only write to the leader, but can read from any follower, this method works well in the web, where there are many more reads than writes.

### Adding new followers
1. Take a consistent snapshot of the leader’s database at some point in time.
2. Copy the snapshot to the new follower node.
3. The follower connects to the leader and requests all the data changes that have happened since the snapshot was taken. 
	- This requires that the snapshot is associated with an exact position in the leader’s replication log. 
		- That position is called the *log sequence number* in Postgres.
4. When the follower has processed the backlog of data changes since the snapshot, we say it has caught up. It can now continue to process data changes from the leader as they happen.

### Failover
On its local disk, each follower keeps a log of the data changes it has received from the leader. If a follower crashes and is restarted, or if the network between the leader and the follower is temporarily interrupted, the follower can recover quite easily. It simply needs to connect to the leader and request all the data changes that occurred during the time when the follower was disconnected

However, things are much trickier if the leader fails. Three things must happen (this process is called *failover*): 
1. one of the followers needs to be promoted to be the new leader. 
2. clients need to update who they send their write requests to. 
3. the remaining followers must be aware of who the new leader is.

#### Failover comes with some wrinkles:
- If asynchronous replication is used, the new leader may not have received all the writes from the old leader before it failed. If the former leader rejoins the cluster after a new leader has been chosen, what should happen to those writes? The new leader may have received conflicting writes in the meantime. The most common solution is for the old leader’s unreplicated writes to simply be discarded, which may violate clients’ durability expectations.
- what if our leader database has an incrementing strategy for assigning IDs? Now, when the leader fails, unless the follower-to-be-leader is perfectly up to date, it will start assigning IDs that have already been assigned by the original leader.
- we could wind up in a situation where two former followers both think they are the new leader (a situation called *split brain*)

### Implementing a Replication Log
a replication log is a stream of database write events, produced by the leader as it processes transactions. The followers apply that stream of writes to their own copy of the database and thus end up with an accurate copy of the same data. The events in the replication log describe the data changes that occurred.

There are several different replication methods
- statement-based replication
- WAL shipping

#### Statement-based replication
The leader logs every write request (e.g. INSERT, UPDATE, DELETE) that it executes and sends that statement log to its followers. Then each follower executes those commands as if they had come from the client directly.

Drawbacks:
- what happens with non-deterministic functions like `now()` and `rand()`?
- if IDs are incremented, or if a statement depends on existing data, they must be executed in the exact same order on each replica
- statements that have side effects (e.g. [[triggers|pg.lang.triggers]], [[functions|pg.lang.func.custom]]) may result in different side-effects on each replica, unless they are deterministic.

#### Write-ahead log (WAL) shipping
In the normal order of business, every write is appended to a [[WAL|db.acid.atomicity#write-ahead-logging-wal,1]].
- instead of the leader simply writing the WAL to disk, it also sends it to all followers. This allows each follower to build a copy of the exact same data structures as found on the leader.
- used in [[pg]]

Drawback:
- the log describes the data on a very low level: a WAL contains details of which bytes were changed in which disk blocks, meaning replication is closely coupled to the storage engine (A different strategy using a *logical log* exists to solve this issue of coupling. This is [supported by Postgres](https://www.postgresql.org/docs/10/logical-replication.html)).
	- If the database changes its storage format from one version to another, it is typically not possible to run different versions of the database software on the leader and the followers.
	- the only way to solve this (without downtime) is to upgrade all the followers, then perform a *failover*, resulting in one of the followers becoming the new leader. For this to work, the replication protocol must support version mismatching like this. Typically, WAL shipping doesn't allow this.

## Multi-Leader based replication
In this setup, replication still happens in the same way as *single-leader replication*: each node that processes a write must forward that data change to all the other nodes.
- The key difference is that each leader simultaneously acts as a follower to the other leaders.

Due to the added complexity of setup, we use this configuration mainly when we have multiple datacenters. In this case, each datacenter would have its own leader.
- each datacenter implements single-leader replication, but between datacenters, each leader replicates its changes to the leaders in other datacenters.

With this replication strategy, each datacenter can continue operating independently of the others

BDR is a Postgres tool for implementing this replication strategy.

anal: consider an [[offline-first|general.arch.offline-first]] application like a calendar. We can have a calendar on our laptop, tablet and mobile, and each acts as a leader (ie. it accepts write requests). But we also need some way to sync data between devices (ie. an asynchronous multi-leader replication process)
- From an architectural point of view, each device is a datacenter with extremely unreliable connection between them (because of the offline-first nature)

### Write Conflicts
This strategy has a major downside: the same data may be concurrently modified in two different datacenters, requiring us to resolve those write conflicts.
- because of this, the multi-leader replication strategy is dangerous and should be avoided if possible.
- write conflicts don't occur in single-leader replication, since writes are applied sequentially.
	- either the database would lock and wait for the first data modification to be written, or the second transaction would simply fail and require the user to retry the write.

In multi-leader, if each follower simply applied writes in the order that it saw them, the database would end up in an inconsistent state. Conflicts must be solved in a convergent way, rather than sequential.

conflict resolution usually applies at the level of an individual row or document, not for an entire transaction
- Thus, if you have a transaction that atomically makes several different writes, each write is still considered separately for the purposes of conflict resolution.

Consider that conflicts don't necessarily have to be about writing different data to the same row. What if we had a meeting room booking system, and 2 different people booked the same room (which occurred because they each wrote to their own respective leaders)

In the event of write conflicts, there are a few strategies to resolve them:
- out of the contenders, designate some field value to be the determination factor. This could be a `createdAt` timestamp (we simply take the latest one), or we might assign each write a unique UUID. In the case of conflict, we simply accept the one with the highest ID.
	- this approach is popular, but it is naturally prone to data loss.
- give each replica a unique ID, and pre-designate replicas with the higher ID as the winner.
	- also implies data loss.
- merge the values together.
	- if we're talking about an object, we might be able to do this cleanly. But if we're talking a string, we might have to concatenate them.
- record the conflict in a separate data structure that preserves all information, and write application code that resolves the conflict at some later time (perhaps by prompting the user to handle the conflict).
- custom logic (most multi-leader replication tools allow us to write application code to implement custom conflict-resolution logic). This code gets executed on either read or write.
	- *on write* - As soon as the database system detects a conflict in the log of replicated changes, it calls the conflict handler.
	-	*on read* - When a conflict is detected, all the conflicting writes are stored. The next time the data is read, these multiple versions of the data are returned to the applica‐ tion. The application may prompt the user or automatically resolve the conflict, and write the result back to the database.
		- this is how CouchDB works.

## Leaderless Replication
In a leaderless replication scheme, any replica can accept writes from clients directly.

General replication then happens in one of two ways:
1. the client directly sends its writes to several replicas
2. a coordinator node does this on behalf of the client
	- Unlike a leader database, that coordinator does not enforce a particular ordering of writes.

Leaderless replication is also suitable for multi-datacenter operation, since it is designed to tolerate conflicting concurrent writes, network interruptions, and latency spikes.

Leaderless replication could be summarized as *"the database will do as much as it can, and if it runs into an error, it won’t undo something it has already done"*— so it’s the application’s responsibility to recover from errors

used by [[DynamoDB|aws.svc.dynamo]], Cassandra, Riak, [[CouchDB|couchdb]]

* * *

# Synchronous / Asynchronous Replication
In relational databases, this is often a configurable option; other systems are often hardcoded to be either one or the other.

Whether a replication scheme is synchronous or asynchronous has a profound effect on the system behavior when there is a fault.

## Synchronous
In a synchronous flow, the leader will wait until the follower has confirmed that it received the write before reporting success to the user, and before making the write visible to other clients.

The advantage of synchronous replication is that the follower is guaranteed to have an up-to-date copy of the data that is consistent with the leader. If the leader sud‐ denly fails, we can be sure that the data is still available on the follower. The disad‐ vantage is that if the synchronous follower doesn’t respond (because it has crashed, or there is a network fault, or for any other reason), the write cannot be processed. The leader must block all writes and wait until the synchronous replica is available again.
- therefore, it is impractical for all followers to be synchronous: any one node outage would cause the whole system to grind to a halt.
- In practice, if you enable synchronous replication on a database, it usually means that one of the followers is synchronous, and the others are asynchronous. If the synchronous follower becomes unavailable or slow, one of the asynchronous followers is made synchronous. This guarantees that you have an up-to-date copy of the data on at least two nodes: the leader and one synchronous follower.

## Asynchronous
In an asynchronous flow, the leader sends the message, but doesn’t wait for a response from the follower.

Asynchronous replication can be fast when the system is running smoothly, but there are complications to consider when replication lag increases and servers fail.
- If a leader fails and you promote an asynchronously updated follower to be the new leader, recently committed data may be lost.

Often, leader-based replication is configured to be completely asynchronous. 
- In this case, if the leader fails and is not recoverable, any writes that have not yet been replicated to followers are lost. This means that a write is not guaranteed to be durable, even if it has been confirmed to the client. 
- However, a fully asynchronous configuration has the advantage that the leader can continue processing writes, even if all of its followers have fallen behind.

if an application reads from an asynchronous follower, it may see outdated information if the follower has fallen behind
- if you run the same query on the leader and a follower at the same time, you may get different results, because not all writes have been reflected in the follower (due to *replication lag*). This is just a temporary state, since the followers will eventually catch up. This effect is known as *eventual consistency*

The value of asynchronous replication is gets higher:
- as the number of followers increases
- as our followers get more geographically distributed.

### Consistency models for dealing with replication lag
When implementing an asynchronous strategy, there are some things to be aware of. Depending on our application, we might not necessarily care too much. 
- ex. in Facebook's newsfeed, it doesn't really matter if all the latest posts are actually there. As long as it's eventual, it's fine.

#### read-after-write consistency
What happens if a user performs some action that writes some data to the database (ie. to the leader node), but by the time the user goes to view that data in the UI, the replication of data hasn't taken place yet between the leader and the follower that is handing the GET request? We need a way of implementing read-after-write consistency.

There are different strategies to implement read-after-write consistency, depending on what we're doing:
- when user is reading data that they recently modified, allow them to read directly from the leader. The issue is that we need to know if data has been modified without first querying for it. Therefore, we can make a simple rule: if the data is modifiable by the user, read from the leader; otherwise, read from a follower.
	- ex. in a social media website, you can only edit your own information. Therefore, if you are querying for your own data, always read from the leader.
- track the time of the last update and, for one minute after the last update, make all reads from the leader. You could also monitor the replication lag on followers and pre‐ vent queries on any follower that is more than one minute behind the leader.
- The client can remember the timestamp of its most recent write, then the system can ensure that the replica serving any reads for that user reflects updates at least until that timestamp. If a replica is not sufficiently up to date, either the read can be handled by another replica or the query can wait until the replica has caught up.

#### Monotonic reads
Imagine user1 writes a comment on a post. The leader node replicates it instantaneously to follower1, but experiences lag in replicating it to follower2. User2 then logs in and reads the data from follower1, so it sees the comment from user1. Then, User2 refreshes the page, but this time the data is read from follower2. Due to the lag, User2 no longer sees the comment from user1.
- *Monotonic reads* is a guarantee that this anomaly doesn't happen.
- we achieve this by making sure that each user always makes their reads from the same replica

#### Consistent Prefix Reads
Imagine user1 comments on a post saying "how is the weather in Chicago?", then user2 writes a comment in response "pretty good". Now, imagine that user3 sees this post with the comments (via the data provided from followers). The comment from user2 gets replicated with little lag, but the comment from user1 experiences a lot of lag. The result may be that user3 sees these comments out of order.
- *Consistent prefix reads* is a guarantee that this anomaly doesn't happen.
	- This guarantee says that if a sequence of writes happens in a certain order, then anyone reading those writes will see them appear in the same order.

This is a particular problem in partitioned ([[sharded|db.distributed.partitioning.sharding]]) databases
- If the database always applies writes in the same order, this anomaly doesn't happen
- But, in many distributed databases, different partitions operate independently, so there is no global ordering of writes: when a user reads from the database, they may see some parts of the database in an older state and some in a newer state.

* * *

# Conflict Resolution Strategies
### Last-Write Wins (LWW)
Widely used in both multi-leader replication and leaderless databases 

Problems with LWW:
- Because of [[clock|hardware.cpu.clock]] drift between nodes, a node with a lagging clock is unable to overwrite values previously written by a node with a fast clock until the clock skew between the nodes has elapsed. This can cause arbitrary amounts of data to be silently dropped without ever alerting the application.
- LWW cannot distinguish between writes that occurred sequentially in quick succession (e.g. client B’s increment definitely occurs after client A’s write) and writes that were truly concurrent (neither writer was aware of the other).