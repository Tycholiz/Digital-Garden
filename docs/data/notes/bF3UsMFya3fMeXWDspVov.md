
PostgresXL is a horizontally scalable SQL database cluster
- It is equipped to handle write-intensive workloads that depend on atomic transactions which are potentially distributed
	- OLTP
- PostgresXL allows you to either partition tables across multiple nodes, or replicate them.
	- Partitioning (or distributing) tables allows for write scalability across multiple nodes as well as massively parallel processing (MPP) for Big Data type of workloads.
		- MPP is using many computers to perform a set of coordinated computations in parallel

## Components of Postgres-XL
### Global Transaction Monitor
- ensures cluster-wide transaction consistency
	- responsible for issuing transaction ids and snapshots as part of its Multi-version Concurrency Control.

### Coordinator
- manages the user sessions and interacts with GTM and the data nodes
	- parses and plans queries, and sends down a serialized global plan to each of the components involved in a statement.

### Data node
- where the actual data is stored.
	- The distribution of the data can be configured by the DBA.
- warm standbys of the data nodes can be configured to be failover-ready.
