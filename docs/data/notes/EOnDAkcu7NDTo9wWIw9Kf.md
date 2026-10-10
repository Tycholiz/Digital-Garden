
### How Postgres handles Transactions and Concurrency
Postgres handles these 2 problems with MVCC (multi-version concurrency control)
- When you update or delete a row, Postgres doesn't actually remove the row. When you do an UPDATE or DELETE, the row isn't actually physically deleted. For a DELETE, the database simply marks the row as unavailable for future transactions, and for UPDATE, under the hood it's a combined INSERT then DELETE, where the previous version of the row is marked unavailable. These new versions of rows are generally referred to as the "live" rows, and the older versions are referred to as "dead" rows.

## UE Resources
[Concurrency control](https://www.postgresql.org/docs/current/mvcc-intro.html)
