
in practice, one database’s implementation of ACID does not equal another’s implementation. 
- when a system claims to be “ACID compliant,” it’s unclear what guarantees you can actually expect.
- systems that don't meet the ACID criteria are called BASE (Basically Available, Soft state, and Eventual consistency)

"As a developer, we should think of a database as something that is ACID-compliant." —Dmitri Fontaine

[[Transactions|db.strategies.transaction]] are one way to achieve full ACID-compliance.
- except for [[durability|db.acid.durability]], which is sort of its own thing.

* * *

From the Postgres-XL docs, here is an implication of it being fully ACID:
*When you start a transaction or query in Postgres-XL, you’ll see a consistent version of your data across the entire cluster. While you are reading your data on one connection, you could be updating the same table or even row in another connection without any locking. Both connections are working with their own versions of the rows, thanks to global transaction identifiers and snapshots.  Readers do not block writers and writers do not block readers.*

## UE Resources
http://ithare.com/databases-101-acid-mvcc-vs-locks-transaction-isolation-levels-and-concurrency/
