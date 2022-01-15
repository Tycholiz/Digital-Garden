
Transactions have a status to them. They might be in a "running" state, or they might be in a "failed" state.

If a function call has failed within a transaction block and we try to commit, we will instead rollback. Observe:
```sql
BEGIN;
SELECT 1/0; -- ERROR: division by zero
COMMIT; -- error detected, ROLLBACK executed
```

Postgres transactions are isolated to individual clients, meaning that the same client must be the one to execute all code in a transaction block.
- In `node-postgres`, we cannot use `pool.query` because of this, (spec:) because a pool may be seen as multiple different clients to the postgres server.

In Postgres
- To execute a transaction in Postgres, use BEGIN / COMMIT / ROLLBACK
- in Postgres, DDL commands are transactional, except when the commands are "high-caliber", such as creating and deleting DATABASE, TABLESPACE, CLUSTER
- PostgreSQL supports multi-level transactions on save points level
	- If an error occurs inside a transaction, PostgreSQL rolls back the whole transaction but demands a command to complete the current transaction (COMMIT, ROLLBACK, ABORT)
- Postgres treats every SQL statement as being executed within a transaction implicitly. If we do not issue a BEGIN command, then each statement will be surrounded with BEGIN..COMMIT

Transactions come with a computational load. For example, if we are populating a database for the first time, we should disable AUTOCOMMIT, and instead only commit all inserts one time. This has the added benefit of being able to rollback the inserts if there is some sort of error.
