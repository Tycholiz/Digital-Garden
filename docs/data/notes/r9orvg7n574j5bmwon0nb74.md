
MVCC aims to solve the problem of concurrency by keeping multiple copies of each data item.
- therefore, each user connected to the database sees a snapshot of the database at a particular instant in time, and any changes made by a single writer will not be seen by anyone else until the transaction has been committed.

An MVCC database doesn't actually overwrite database records when it updates them. Instead, it creates a newer version of the item.
- therefore, it is inherent in MVCC that multiple versions of a single item are stored.
- this introduces a new problem: when do we delete objects that are *definitely* obsolete?

An MVCC database also allows each transaction's read operations to read objects of a previous revision

When a [[transaction|db.strategies.transaction]] reads from a consistent [[snapshot|db.acid.isolation#snapshot-isolation,1:#*]] in an MVCC database, it ignores writes that were made by any other transactions that hadn’t yet committed at the time when the snapshot was taken.