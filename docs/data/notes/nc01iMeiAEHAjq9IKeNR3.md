
`coalesce` accepts unlimited params, and always returns the first non-null value
- In this way, you can think of the final param as the default value.

Embracing `null` allows us to easily inline defaults
- You can chain a bunch of inputs and it returns the first non-null value
- ex. imagine having a result set that shows us how much revenue was earned each year since 2010. We should define a column `coalesce(income, 0) as income` in our select statement, which will return us a 0 if there is no income for the relevant year. This is a nicer alternative than to just omitting rows without data.
- ex. Lets say you have an input parameter with a default, you can the just wrap the usages of that parameters like this: coalesce (_parameter, default_value) = whatever it tests against
- ex. Imagine we have a postgres function that accepts 2 args: a `start_date` and an `end_date`, and returns the amount of time that an employee worked at a company. If they currently work there still, the client passes `null` for the value of the `end_date`. In our function, we would have a smart failover that defaults the null value to `now()`.
- ex. imagine we have a column `discount` that defaults to `null`. We want to use this `discount` column when determining subtotal, so we need to use coalesce to provide us with a default value of 0:
```sql
SELECT
	product,
	(price - COALESCE(discount,0)) AS net_price
```
