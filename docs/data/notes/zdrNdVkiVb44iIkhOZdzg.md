
# For Loops
```sql
do
$$
declare
	rec record;
begin
	for rec in select title, length
			from films
			where length > 50
			order by length
	loop
		raise notice '% (%)', rec.title, rec.length;
	end loop;
end;
$$
```
