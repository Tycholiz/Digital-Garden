
Atomicity describes what happens if a client wants to make several writes, but a fault occurs after some of the writes have been processed
- ex. a process crashes, a network connection is interrupted, a disk becomes full, or some integrity constraint is violated.
- If the writes are grouped together into an atomic transaction, and the transaction cannot be completed (committed) due to a fault, then the transaction is aborted and the database must discard or undo any writes it has made so far in that transaction.

The ability to abort a transaction on error and have all writes from that transaction discarded is the defining feature of ACID atomicity.
- therefore we could think of this concept as *abortability*

Imagine a situation we want to update the schema of our database (eg introduce a new table or modify an existing one). If our system crashed in the middle of the update and our database didn't have Atomicity, then the result would be a mix between v1 and v2.

In Postgres, Atomicity is provided through write-ahead logs. Shadow Paging is another technique to provide atomicity.

### Transaction
- logically clumping together multiple database interactions (CRUD) as if they were one action
	- ex. in a balance sheet, we want to treat the asset change that corresponds to the liability+shareholer's equity change as atomic. Without transactions, if we were to make one change and for some reason the second change fell over, we would have an imbalanced balance sheet. At least with Transactions, we can be guaranteed that this would never occur
- When an execution of transaction is interrupted, the transaction is not executed at all.

### Write-Ahead Logging (WAL)
- Before the results of a transaction are written to the database, the changes are first recorded in a log. This log is written to stable storage to ensure that the whole complete file is saved. Only once this log is known to be securely stored is the change made to the database. 
	- ex. imagine the machine hosting the database lost power midway through performing some operation. When the machine boots back up, we can use WAL to determine what atomic changes were *supposed* to have been made, and compare that to what actually changed. With this information, we can determine whether or not we should rollback. This guarantees both atomicity and durability.
- the log describes the data on a very low level: a WAL contains details of which bytes were changed in which disk blocks.
