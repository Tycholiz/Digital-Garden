
A partial index is an index built over a subset of a table, and therefore only contains entries for that subset.
- The subset is determined by a conditional expression given by us (called the predicate).

## Purpose
- To avoid indexing common values
    - Since a query searching for a common value (ie. one that occurs for more than a few percent of all table rows) will not use the index anyway, there is no point in keeping those rows in the index at all. Doing this reduces the size of the index, increasing the speed of queries that do ue the index.
