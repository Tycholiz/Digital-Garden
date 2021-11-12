
# Atomic
- The idea that if we have multiple things to do in a single transaction, we can roll it back as a chunk
	- Imagine a transaction consists of 2 inserts and 1 update. The fact that we have atomicity means that we can rollback that whole transaction. This distinction is even more important when you consider multi-table transactions, which MongoDB does not offer.
		- ex. In old versions of MongoDB if you needed to remove an item from inventory and add it to someone's order at the same time, you could not.
	- Imagine a situation we want to update the schema of our database (eg introduce a new table or modify an existing one). If our system crashed in the middle of the update and our database didn't have Atomicity, then the result would be a mix between v1 and v2.
- In Postgres, Atomicity is provided through write-ahead logs. Shadow Paging is another technique to provide atomicity.

### Transaction
- logically clumping together multiple database interactions (CRUD) as if they were one action
	- ex. in a balance sheet, we want to treat the asset change that corresponds to the liability+shareholer's equity change as atomic. Without transactions, if we were to make one change and for some reason the second change fell over, we would have an imbalanced balance sheet. At least with Transactions, we can be guaranteed that this would never occur
- When an execution of transaction is interrupted, the transaction is not executed at all.

### Write-Ahead Logging (WAL)
- Before the results of a transaction are written to the database, the changes are first recorded in a log. This log is written to stable storage to ensure that the whole complete file is saved. Only once this log is known to be securely stored is the change made to the database. 
	- ex. imagine the machine hosting the database lost power midway through performing some operation. When the machine boots back up, we can use WAL to determine what atomic changes were *supposed* to have been made, and compare that to what actually changed. With this information, we can determine whether or not we should rollback. This guarantees both atomicity and durability.
