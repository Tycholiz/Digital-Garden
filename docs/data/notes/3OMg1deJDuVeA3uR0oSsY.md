
In contrast to the way a view is queried, a materialized view actually takes up storage space, making it a proper table (and not just a conceptual table). When you query a materialized view, the result set is cached and stored (or, materialized) as a sort of pseudo table
- can be thought of as a snapshot of a query set
- makes queries more efficient at the expense of space

The difference is that a materialized view is an actual copy of the query results, written to disk, whereas a virtual view is just a shortcut for writing queries.

When the underlying data changes, a materialized view needs to be updated, because it is a denormalized copy of the data.
- The database can do that automatically, but such updates make writes more expensive, which is why materialized views are not often used in [[OLTP|db.olap-oltp]] databases.