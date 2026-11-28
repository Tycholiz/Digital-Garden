
Redis Clusters enable us to scale [[horizontally|deploy.scaling#horizontal-scaling-aka-scaling-out,1:#*]]

When we run Redis in a cluster, data is automatically sharded across multiple Redis nodes.

As an added benefit, running as a cluster also gives a degree of availability during partitions, giving it the ability to continue in the event that there is a fault in one or more of the nodes.

Redis Cluster uses a master-replica model where every hash slot has from 1 (the master itself) to N replicas (N-1 additional replica nodes).

Redis Cluster does not guarantee strong consistency, due to the fact that it uses asynchronous replication.
- In practical terms this means that under certain conditions it is possible that Redis Cluster will lose writes that were acknowledged by the system to the client.
- ex.
  1. Your client writes to the master B.
  2. The master B replies OK to your client.
  3. The master B propagates the write to its replicas B1 and B2, but crashes before it is able to replicate to B3, resulting in B3 being behind, yet the client thinking everything has been properly replicated.

### Connections
Each cluster requires 2 open [[TCP|protocol.TCP]] connections:
1. a Redis TCP port used to serve clients, e.g., 6379, 
2. a second port known as the cluster bus port
  -  By default, the cluster bus port is set by adding 10000 to the data port (e.g., 16379);

The cluster bus port is for node-to-node communication and is used for failure detection, configuration update, failover authorization, and so forth.

Both ports need to be open to the firewall, otherwise Redis cluster nodes will be unable to communicate.

### Sharding
Each key (ie. each value to be stored in Redis) is sharded and grouped according to the *hash slot* they belong to.
- there are 16384 individual hash slots in Redis Cluster.
- each cluster node is responsible for a subset of the hash slots.

Identical values within `{}` are guaranteed to be in the same hash slot because they share the same hash tag
- ex. the keys `user:{123}:profile` and `user:{123}:account` will live in the same hash slot, since they are from the same user.
- as a result of this, we can operate on these 2 keys in the same multi-key operation.