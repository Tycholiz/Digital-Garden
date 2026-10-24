
Durability is the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes.
- in a single-node system, durability means that the data has been written to disk, 
- in a multi-node system, durability means that the data has been copied to some number of nodes.
- to provide a durability guarantee, a database must wait until these writes or replications are complete before reporting a transaction as successfully committed

A second layer of durability is usually provided through WALs write-ahead logs, which allows recovery in the event that the data structures on disk are corrupted.
