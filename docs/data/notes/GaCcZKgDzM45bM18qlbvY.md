
Postgres has multiple types of [[index|db.strategies.index]]: [[B-tree|general.lang.data-structs.tree.B]], Hash, GiST, SP-GiST and GIN
- B-tree (balanced tree) is default, and is appropriate for most use-cases

Using the `CREATE INDEX` command, we are creating a secondary index. Primary indexes are created by default via the primary key.
- secondary joins are crucial for performing JOINs effectively.

Whether to create a single-column index or a multicolumn index, take into consideration the column(s) that you may use very frequently in a query's WHERE clause as filter conditions.
- ie. add an index on the columns that we typically use WHERE on

Indexing is often thought of as a data modeling activity. When using PostgreSQL, some indexes are necessary to ensure data consistency (the [[C|db.acid.consistency]] in ACID). Constraints such as UNIQUE, PRIMARY KEY or EXCLUDE USING are only possible to implement in PostgreSQL *with* a backing index. When an index is used as an implementation detail to ensure data consistency, then the indexing strategy is indeed a data modeling activity.
- In all other cases, the indexing strategy is meant to enable methods for faster access methods to data. Those methods are only going to be exercised in the context of running a SQL query

In the case of UNIQUE, PRIMARY KEY and EXCLUDE USING, the reason why PostgreSQL needs an index is because it allows the system to implement visibility tricks with its MVCC implementation 
- this fact is what prevents these 2 (concurrently ran) transactions (which should conflict on UNIQUE constraint) from being accepted:
```sql
t1> insert into test(id) values(1);
t2> insert into test(id) values(1);
```

### Indexing comes at a cost
Determining *what not* to index is probably more important than determining *what* to index
- An index duplicates data in a specialized format made to optimise a certain type of searches. When we `COMMIT;`, every change that is made to the main tables of your schema must have made it to the indexes too.
	- As a consequence, each index adds write costs to your DML queries: INSERT, UPDATE and DELETE now have to maintain the indexes too, and in a transactional way.

Avoid indexes on...
- small tables.
- Tables that have frequent, large batch update or insert operations.
- columns that contain a high number of NULL values.
- Columns that are frequently manipulated should not be indexed.

* * *

[[Index Docs|db.strategies.index]]

# UE Resources
- [Indexes under the hood, using B-trees](https://rcoh.me/posts/postgres-indexes-under-the-hood/)
- [provides a quick-n-dirty test at the bottom of the article, to see impact of indexes on millions of rows](https://www.cybertec-postgresql.com/en/postgresql-now-vs-nowtimestamp-vs-clock_timestamp/)
