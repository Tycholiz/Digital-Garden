
A log is an append-only and totally ordered data structure sequence of changes.

Logs are powerful primitives for building [[distributed|deploy.distributed]] systems.

Many RDBMSs use change logs to improve performance, provide point-in-time recovery (PITR) after a crash, and implement replication.
- ex. Postgres uses Write-ahead Logs (WALs)
- A database change log also effectively allows the creation of a distributed database via replication of data on additional external hosts.

To utilize logs to their fullest, we must understand the key principles of ordering, [[deduplication|storage.deduping]], and checkpointing.
