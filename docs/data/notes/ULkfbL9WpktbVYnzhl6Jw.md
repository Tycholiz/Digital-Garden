
Postgres is a [[DBMS|db.DBMS]].
- "SQL tells the database what information you want, and the DBMS determines the best way to provide it."

Think of postgres as a [[microservice|general.arch.microservice]] that is stateful.
- Further, don’t think of it as a storage layer, but rather as a concurrent data access service. The service is capable of handling data processing. When we think of Postgres only as our data layer, we tend to overlook the power that Postgres can actually bring to the table.
	- anal: think of The Matrix, where Neo awakes in the pod near the start of the movie. If each pod were a piece of data, the mechanical arms that move and manipulate the pods would be Postgres.
		- In actuality, it would be both— the whole system. Limiting our thinking to just the data also limits our thinking in terms of what we consider Postgres to be capable of.

In PG, data consistency is maintained by using a multiversion model ([[db.strategies.concurrency-control.MVCC]]).
- This means that each SQL statement sees a snapshot of data (a database version) as it was some time ago, regardless of the current state of the underlying data.
- [source](https://www.postgresql.org/docs/current/mvcc-intro.html)

### Performance
It's possible to scale Postgres to storing a billion 1KB rows entirely in memory - This means you could quickly run queries against the full name of everyone on the planet on commodity hardware and with little fine-tuning.
- Postgres can easily handle 10,000 insertions per second.

If we have the SQL do the data related heavy lifting, there will often be a net gain in performance. Mostly, it is because round-trip times and latency along with memory and bandwidth resources usage depend directly on the size of the result sets.

It's rarely a mistake to start with Postgres and then switch out the most performance critical parts of your system when the time comes.

# UE Resources
- [Exercises for learning SQL with PG](https://pgexercises.com/)
- [PG simple explanations](https://www.avestura.dev/blog/explaining-the-postgres-meme)
- [Postgres Do's and Don'ts](https://wiki.postgresql.org/wiki/Don't_Do_This)
