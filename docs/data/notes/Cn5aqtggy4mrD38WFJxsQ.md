
WHERE determines which rows will be included in the aggregate calculation
- this show that the WHERE clause must be evaluated before aggregate functions are computed.
```sql
-- WRONG
SELECT city FROM weather WHERE temp_lo = max(temp_lo);

-- CORRECT
SELECT city FROM weather
    WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```
![](/assets/images/2021-03-09-16-30-49.png)
- In contrast with HAVING, WHERE selects input rows before groups and aggregates are computed
	- thus, it controls which rows go into the aggregate computation

- We usually try to keep the where clauses as simple as possible for PostgreSQL in order to be able to use our indexes to solve the data filtering expressions of our queries.
- the OR operator is more complex to optimize for, in particular with respect to indexes.
