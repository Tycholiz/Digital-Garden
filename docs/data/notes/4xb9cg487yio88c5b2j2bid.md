
Transactions are an abstraction layer that allows an application to pretend that certain concurrency problems and certain kinds of hardware and software faults don’t exist.
- A large class of errors is reduced down to a simple transaction abort, and the application just needs to try again.

An application with very simple access patterns, such as reading and writing only a single record, can probably manage without transactions.

Transactions have a status to them. They might be in a "running" state, or they might be in a "failed" state.

Almost all relational databases (and some nonrelational) support transactions.

Transactions come with a computational load. For example, if we are populating a database for the first time, we should disable AUTOCOMMIT, and instead only commit all inserts one time. This has the added benefit of being able to rollback the inserts if there is some sort of error.

Not every application needs transactions, and sometimes there are advantages to weakening transactional guarantees or abandoning them entirely 
- ex. without transactions, we achieve higher performance or higher availability. 
- Some safety properties can be achieved without transactions.

### Transactions to ensure write+read from same replica
Transactions can be used to ensure that multiple operations are performed on the same replica.
- ex. if we have a bulk write operation and a read operation where we read those documents we just created, we can wrap the write and the read in a transaction, which ensures that the read happens on the same replica that we wrote to, and doesn't read from a replica which may not yet have received those updates.

### The distinction between single-object and multi-object transactions
Used properly, transactions refer to multi-object transactions. That is, we can update objects across different tables in a single transaction, and we get an all-or-nothing result. However, there is also the concept of single-object transactions, which is the guarantee for instance that a JSON value being updated will either update the whole thing or nothing at all; we won't end up in a state where only half of the JSON has been updated by the system. This single-object transaction guarantee is implemented by almost every database.
- this idea of single-object transaction covers the whole document when updating in a [[document database|db.type.document]].

When a transaction is started, it is given a unique, always-increasing transaction ID (txid). Whenever a transaction writes anything to the database, the data it writes is tagged with the transaction ID of the writer.

### Transactions in Distributed Databases
Transactions (multi-object) have mostly been abandoned by distrubuted databases, since they are difficult to implement across partitions. They also get in the way of high-availability and performance, which is normally a staple of [[nosql]] databases.

There is nothing that fundamentally prevents transactions in a distributed database. However, the costs outweigh the benefits, and the thought is that we can achieve all the transactional integrity we need with single-object operations.

Even if we need to keep different objects in sync, we can still achieve this without transactions. However, error handling becomes much more complicated without [[atomicity|db.acid.atomicity]], and the lack of [[isolation|db.acid.isolation]] can cause concurrency problems.
