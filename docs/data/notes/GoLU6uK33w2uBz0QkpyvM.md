
### SELECT INTO
- SELECT INTO creates a new table and fills it with data computed by a query.
- The data is not returned to the client, as it is with a normal SELECT.
- The new table's columns have the names and data types associated with the output columns of the SELECT.

#### with `record` type
```sql
do
$$
declare
	rec record;
begin
	-- select the film
	select film_id, title, length
	into rec
	from film
	where film_id = 200;

	raise notice '% % %', rec.film_id, rec.title, rec.length;

end;
$$
language plpgsql;
```
