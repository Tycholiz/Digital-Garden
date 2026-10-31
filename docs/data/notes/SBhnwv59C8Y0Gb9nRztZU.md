
Isolation determines the extent to which items involved in a single [[transaction|db.strategies.transaction]] are visible to everything outside the transaction.
- a lower isolation level increases the ability of many users to access the same data at the same time, but increases the number of concurrency effects (such as dirty reads or lost updates) users might encounter. 
- a higher isolation level reduces the types of concurrency effects that users may encounter, but requires more system resources and increases the chances that one transaction will block another.

Isolation is the idea that if we have multiple things to do in a single transaction, we can roll it back as a chunk.
- Imagine a transaction consists of 2 inserts and 1 update. The fact that we have atomicity means that we can rollback that whole transaction. This distinction is even more important when you consider multi-table transactions, which MongoDB does not offer.
- ex. In old versions of MongoDB if you needed to remove an item from inventory and add it to someone's order at the same time, you could not.
- ex. in multi-threaded programming, if one thread executes an atomic operation, that means there is no way that another thread could see the half-finished result of the operation. The system can only be in the state it was before the operation or after the operation, not something in between.
A transaction in process and not yet committed must remain isolated from any other transaction.

Isolation is about having multiple [[concurrently|general.concurrency]] executing transactions that are isolated from each other.
- The database ensures that when the transactions have committed, the result is the same as if they had run *serially* (one after another), even though in reality they may have run concurrently

Isolation is the opposite side of atomicity.
- while we are doing our queries, are we allowed to see what is happening concurrently in the rest of the system?
	- ex. what if we want to make a backup with `pg_dump` that needs to run for several hours? That backup needs to be a consistent snapshot of the production database. If during the backup someone is doing inserts, we don't want these to be in the backup, since we want a snapshot that doesn't move.		- to do this, postgres uses an isolation mode that prevents this from happening.	

isolation can be implemented using a lock on each object (allowing only one thread to access an object at any one time)

In theory, isolation should make your life easier by letting you pretend that no concurrency is happening.

## Levels of isolation
By increasing levels of isolation, we are able to prevent some race conditions from happening.

Higher levels of isolation come with a performance cost that in most cases aren't worth it. It’s therefore common for systems to use weaker levels of isolation, which protect against some concurrency issues, but not all. 

### Read committed
This is the most basic level of transaction isolation.

It makes two guarantees:
1. When reading from the database, you will only see data that has been committed (no dirty reads).
	- if a transaction has written some data to the database, but the transaction has not yet committed or aborted, and another transaction can see that uncommitted data, that is called a *dirty read*.
2. When writing to the database, you will only overwrite data that has been committed (no dirty writes).
	- if 2 transactions concurrently try to update the same object in a database, and second write overwrites an uncommitted value from the first write, that is called a *dirty write*. To prevent this, the second write must be delayed until the first transaction has completed.
	- databases prevent dirty writes by using row-level locks: when a transaction wants to modify a particular object (row or document), it must first acquire a lock on that object, and must only release it when the transaction has completed. 

Read committed isolation does not prevent race conditions occurring when 2 different users are trying to increment the same counter simultaneously.

Read committed is a popular isolation level, and is the default setting in Postgres.

### Snapshot isolation
Snapshot isolation is a guarantee that all reads made in a transaction will see a consistent snapshot of the database, and the transaction itself will successfully commit only if no updates it has made conflict with any concurrent updates made since that snapshot.
- in practice it reads the values resulting from the last committed transaction that existed at the time it started

Snapshot isolation has the mantra "readers never block writers, and writers never block readers".

The idea is that each transaction reads from a consistent snapshot of the database— that is, the transaction sees all the data that was committed in the database at the start of the transaction. Even if the data is subsequently changed by another transaction, each transaction sees only the old data from that particular point in time.
- snapshot isolation is therefore considered necessary for long-running, read-only queries such as backups and analytics. 

Allows better performance than serializability, yet still avoids most of the concurrency anomalies that serializability avoids.

implementations of snapshot isolation typically use write locks to prevent dirty writes

From a performance point of view, a key principle of snapshot isolation is readers never block writers, and writers never block readers. 
- therefore, reads do not require any locks.

Because it maintains several versions of an object side by side, this technique is known as multiversion concurrency control (MVCC).

Snapshot isolation is usually implemented by [[MVCC|db.strategies.concurrency-control]].

used by Postgres (called *repeatable read*), Mongo, MySQL

### Serializability
Serializable isolation means that the database guarantees that transactions have the same effect as if they ran serially (i.e., one at a time, without any concurrency)
- by doing this, all possible race conditions are avoided.

Serializability means each transaction can pretend that it is the only transaction running on the entire database. 

Serializable isolation is rarely used because it carries a performance penalty.

There are 3 principal techniques to implementing serializability:
1. Literally execute transactions in a serial order
	- possible when the entire active dataset can reside in [[memory|hardware.ram]]
	- used in [[Redis]]
2. Two-phase locking
3. Optimistic concurrency control techniques such as serializable snapshot isolation

In Postgres, Serializability is implemented as *serializable snapshot isolation* (SSI), which provides full serializability, but has only a small performance penalty compared to snapshot isolation. 
- optimistic concurrency control technique. Optimistic in this context means that instead of blocking if something potentially dangerous happens, transactions continue anyway, in the hope that everything will turn out all right. When a transaction wants to commit, the database checks whether anything bad happened (i.e., whether isolation was violated); if so, the transaction is aborted and has to be retried. Only transactions that executed serializably are allowed to commit.

## Strategies
### Record Locking
