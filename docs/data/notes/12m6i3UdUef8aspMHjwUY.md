
The group by clause introduces Aggregates (aka Map/Reduce) in Postgres, which enables us to map our rows into different groups and then reduce the result set into a single value.

GROUP BY allows us to divide rows returned from SELECT into groups. It is only applicable to aggregate functions
- GROUP BY groups rows sharing a property so that an aggregate function can be applied to each group
	- for each group, we can apply an aggregate function, ex `SUM()`

Similar to JOINs in the sense that it is about "bringing related data to the same place", which is done by grouping records by some key.

GROUP BY allows implementing much the same thing than `map`/`reduce` do in Javascript
- ie. map your data into different groups, and in each group reduce the data set to a single value.

GROUP BY produces a new table reference with only as many columns as are listed after the GROUP BY clause
- Note that other columns may still be available as arguments of aggregate functions
- ex. `GROUP BY "nugget".id, "nugget".title` will reduce the number of available columns in all subsequent logical clauses (inc. SELECT) to 2.
	- This is the syntactical reason why you can only reference columns from the GROUP BY clause in the SELECT clause.

If we simply GROUP BY an id and don't apply any aggregate function, then the functionality will be the same as DISTINCT (remove duplicate rows)
	- this is why GROUP BY only really becomes valuable once we add an aggregate function

#### Multiple columns in GROUP BY
`GROUP BY X` means put all those with the same value for X in the one group
`GROUP BY X, Y` means put all those with the same values for both X and Y in the one group.

## Examples
### Average salary by position
- ex. imagine we have an `employee` table and we are interested in average salaries. Naturally, we have different employee positions, so if we ran the aggregate function AVG, we would get back an average of every salary:
```sql
SELECT avg(salary)
FROM "employee"
```
since the average of all salaries is not that interesting, we want to get the average of salaries GROUPed BY position
```sql
SELECT position, avg(salary)
FROM "employee"
GROUP BY position
```
- note: if we didn't have the GROUP BY clause here, we would get an error
	- here, GROUP BY basically says "let's see the aggregate data *per* position"
- Sometimes we may see syntax such as `GROUP BY 2`. Here, 2 refers to the second column, and would be equivalent to writing out the actual column name.
