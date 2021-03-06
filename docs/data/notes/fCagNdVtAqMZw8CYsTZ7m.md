
# ACID
"As a developer, we should think of a database as something that is ACID-compliant." —Dmitri Fontaine

* * *

From the Postgres-XL docs, here is an implication of it being fully ACID:
*When you start a transaction or query in Postgres-XL, you’ll see a consistent version of your data across the entire cluster. While you are reading your data on one connection, you could be updating the same table or even row in another connection without any locking. Both connections are working with their own versions of the rows, thanks to global transaction identifiers and snapshots.  Readers do not block writers and writers do not block readers.*
