
The vast majority of slow queries found in the wild are still queries that return way too many rows to the application, straining the network and the server's memory

## Query Plans 
PostgreSQL devises a query plan for each query it receives. Choosing the right plan to match the query structure and the properties of the data is absolutely critical for good performance, so the system includes a complex planner that tries to choose good plans. You can use the EXPLAIN command to see what query plan the planner creates for any query

### EXPLAIN ANALYZE
Tells us things like how is PostgreSQL trying to find the record (using a sequential scan or hash, etc), costs, timing, and performance information.

"EXPLAIN ANALYZE is the most powerful tool at our disposal for understanding and optimizing SQL queries"
- this is a command that accepts a statement such as SELECT ..., UPDATE ..., or DELETE ..., executes the statement, and instead of returning the data provides a query plan detailing what approach the planner took to executing the statement provided.

This is how you might go about using explain to understand run time characteristics of your queries:
```sql
explain (analyze, verbose, buffers)
<query here>;
```

* * *

The combination of Bitmap Index Scan and Bitmap Heap Scan is much more expensive than reading the rows sequentially from the table (a Seq Scan)

Seq Scan nodes often indicate an opportunity for an index to be added, which is much faster to read

[source](https://thoughtbot.com/blog/reading-an-explain-analyze-query-plan)

# Tools
[Explain Depesz: explain analyze made readable](https://explain.depesz.com/)
