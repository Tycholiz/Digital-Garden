
### Scaling PG horizontally vs vertically
The ability to scale horizontally is impacted by the rate of change in the primary database. The primary can be making many changes concurrently. Those changes are serialized, written to the WAL files, and the WAL information is forwarded to the secondary systems. The secondary systems *must* process this data serially. This induces *lag* into the replicant. It will “fall behind” the primary. When will it catch up? It is unpredictable. We know that it will eventually catch up, assuming that the rate of change on the primary is not constant. If you have a system which can tolerate the inherent lag in replication, then Postgres can scale quite well. However, if your workload can not tolerate this lag, you would think it does not scale well at all.

If it is about read-heavy workload then you should just add replicas. Add as many replicas as you need to handle the whole workload. You can balance all the queries across the replicas in the round robin fashion.

If it is about write-heavy workload then you should partition your database across many servers. You can put different tables on different machines or you can shard one table across many machines. In the latter case you can shard a table by a range of the primary key or by a hash of the primary key or even vertically by rows. In each of the cases above you may lose transactionality, so be careful and make sure that all the data changed and queried by a transaction be resided on the same server.

Horizontal scaling in relational databases is hard to achieve because when you have tables (or shards of the same table) across the different cluster nodes, joins usually become very inefficient. Additionally, there is a problem of replication and keeping ACID guarantees while ensuring that all replicas have fresh data

### Streaming Replication
- streaming replication allows us to continuously apply WAL XLOG records to standby servers, so that they are kept current
	- WALs are write-ahead logs and are synonymous with, which are XLOG are transaction logs (X stands for transaction)
