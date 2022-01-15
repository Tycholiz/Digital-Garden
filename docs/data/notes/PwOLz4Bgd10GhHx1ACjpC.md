
# Subquery
- think of subqueries as fluent syntax that's chained by nesting rather than step by step after each other.
	- similar to functional programming and how you read from the inside-out, using the output of the inner function as input for the next outer function 
- subqueries return a subset of data from a larger set of data (which arose from a different query)
	- ie. perform a query to get some data, then immediately perform another query on that data.
- subqueries can be used almost anywhere in an SQL query.
- if a subquery is slow, try and convert it to a derived table, as those are much faster
- Subqueries can usually be executed independently, allowing us to see what each one is doing so we can mentally make substitutions as we are trying to understand an SQL query
- use subqueries any time you want to first query a set of data, which you will then query again on
	- note: in reality it is not as inefficient as it sounds. This is just a simplification
ex. below, if `dr_recommended` is a table with a column called `activity`, then this query will return all activities in both tables, since they will be "doctor recommended" 
select the `type` column go in the `dr_recommended` table
- you can use the WITH clause in place of sub-queries

```sql
select * 
from exercise_logs 
where activity in (
	select type from dr_recommended
);
```
ex. below, we first query for the highest joindate, then out of the subset of data, we query for the joindate, firstname and surname
```sql
select joindate, firstname, surname
from cd.members
where joindate = (select
max(joindate)
from cd.members)
```
