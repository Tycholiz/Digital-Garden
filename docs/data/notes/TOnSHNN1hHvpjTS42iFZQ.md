
# Return value
- functions can return base types (string, int) and composite types (collection of columns), or sets of base types and composite types
- we can return any type from a function. Since the whole row of a table is by definition a composite type, we can specify `returns nugget`. This will specify that the function must return all columns in the `nugget` table.
	- If we don't want to return all columns, we can always use the `ROW` construct to specify which columns we want to include in the row signature
	- The select list order in the query must be exactly the same as that in which the columns appear in the table associated with the composite type.
	- You must typecast the expressions to match the definition of the composite type
- If the function is defined to return a base table, the table function produces a one-column table (with the column named after the function. If the function is defined to return a composite type, the table function produces a column for each attribute of the composite type.
	- using `setof` will return multiple columns
- the type that follows `returns` has to match up with whatever is SELECTed during the query.
	- ex. if we declare `returns bucket`, then we'd better be using `SELECT * FROM bucket`
		- since in this example we are returning a base table, a one-column table is the result.

### Returning a Set
- we can also use `RETURNS TABLE(columns)` syntax to return a set
	- equivalent to using one or more `OUT` parameters plus marking the function as returning SETOF record (or SETOF a single output parameter's type, as appropriate)
	- It is not allowed to use explicit OUT or INOUT parameters with the RETURNS TABLE notation — you must put all the output columns in the TABLE list.

### Output Params
- an alternate way to define a function's signature (inputs and outputs)
- The value of output parameters is that they provide a convenient way of defining functions that return several columns, which is why functions can have multiple outputs
```
CREATE FUNCTION sum_and_product (IN x int, IN y int, OUT sum int, OUT product int)
```
- What has essentially happened here is that we have created an anonymous composite type for the result of the function
	- if we wanted to be more explicit, we could have declared a composite type `sum_prod`, made up of the `sum` column and the `product` column, and declared that the function `returns sum_prod`.

## Set returning function
- *set returning functions* are functions that possibly return more than one row
	- currently, `series generating functions` are the only type of `set returning functions`
- `generate_series(<start>, <stop>, <step>)`
	- because this function returns a result set, we can use the function after FROM
- ex. imagine running:
```
generate_series(date :'start',
			    date :'start' + interval '1 month'
							  - interval '1 day',
				interval '1 day'
) as calendar(entry)
```
which would return a set of dates, starting from `start` (a variable we defined earlier), and increasing by intervals of 1 day.
	- this would be useful if we have a set of data by year, and have some years where there is no data. Instead of skipping those rows, we might want to display zeros instead. When we join this result set with our data, joining on the `date` column will result in `null` for the missing years. Using `coalesce`, we can default all nulls to zero.

# Params
## Named params
2 ways:

```sql
SELECT * FROM app_private.really_create_user(
	username := $1
)
```

or:
```sql
select app_private.really_acquire_book(
	payment_intent_id => $1
)
```
