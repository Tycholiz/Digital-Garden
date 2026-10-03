
Depending on the level of [[isolation|db.acid.isolation]] that the database uses, some types of race conditions can be eliminated.
- *serializable isolation* will protect against all.

## Examples of Race conditions
### Dirty reads
One client reads another client’s writes before they have been committed. The read committed isolation level and stronger levels prevent dirty reads.

### Dirty writes
One client overwrites data that another client has written, but not yet committed.  Almost all transaction implementations prevent dirty writes.

### Read skew (nonrepeatable reads)
A client sees different parts of the database at different points in time. This issue is most commonly prevented with snapshot isolation, which allows a transaction to read from a consistent snapshot at one point in time. It is usually implemented with [[db.strategies.concurrency-control.MVCC]]

### Lost updates
Two clients concurrently perform a read-modify-write cycle. One overwrites the other’s write without incorporating its changes, so data is lost. Some implemen‐ tations of snapshot isolation prevent this anomaly automatically, while others require a manual lock (`SELECT FOR UPDATE`).

### Write skew
A transaction reads something, makes a decision based on the value it saw, and writes the decision to the database. However, by the time the write is made, the premise of the decision is no longer true. Only serializable isolation prevents this anomaly.

### Phantom reads
A transaction reads objects that match some search condition. Another client makes a write that affects the results of that search. Snapshot isolation prevents straightforward phantom reads, but phantoms in the context of write skew require special treatment, such as index-range locks