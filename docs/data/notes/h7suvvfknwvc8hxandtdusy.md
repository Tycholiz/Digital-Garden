
Strong consistency means replication happens synchronously across data nodes
- This means subsequent read operations must all return the same latest data, no matter which replica was queried

### How does it happen?
1. the server (client) executes a write operation against the primary database instance
2. the primary instance propagates the written data to the replica instance
3. the replica instance sends an acknowledgment signal to the primary instance
4. the primary instance sends an acknowledgment signal to the client

The popular use cases of the strong consistency model are the following:
- File systems
- Relational databases
- Financial services such as banking
- Semi-distributed consensus protocols such as two-phase commit (2PC)
- Fully distributed consensus protocols such as Paxos

Google Spanner and Google Bigtable both use strong consistency 

* * *

### Linearizability
Linearizability is a variant of strong consistency and is also known as atomic consistency

The following techniques can be used to implement linearizability:
- single leader to handle both read and write operations
- distributed consensus algorithm such as Paxos
- distributed quorum

The advantages of linearizability are as follows:
- makes a distributed system behave as if the system were non-distributed
- simple for application to use

The disadvantages of linearizability are the following:
- degraded performance
- limited scalability
- reduced availability