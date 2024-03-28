
Imagine we had 2 data sets: cities and capitals. A naive approach might be to create 2 tables called `non_capitals` and `capitals`, then join them together in a UNION
- This approach is bad because problems arise once we need to update multiple rows at once.

Instead, a better solution is to use inheritance:
```sql
 cities (
  name       text,
  population real,
  elevation  int     -- (in ft)
);

CREATE TABLE capitals (
  state      char(2) UNIQUE NOT NULL
) INHERITS (cities);
```
- here, a row of `capitals` will inherit all columns from its parent, and will gain an additional column `state`
- a table can inherit from more than one other table
- using the ONLY clause, we can prevent the query from running over tables below the specified table
	- ex. in the capital cities example, if we query `SELECT * FROM ONLY cities...`, then we prevent capitals from showing up in the result set.
