
#### current_date
spec: when doing "math" on dates that don't have a time attached to them, then midnight is used:
```sql
select count(*)
from app_public.users
where created_at > current_date
and created_at < current_date + interval '1 day'
```
If current_date gets rounded back to midnight, then, the first `where` is saying "give me all the rows created since midnight", and the second `where` is saying "until midnight 24 hours later"

### Extract
using the `extract` function, we can get parts of a date out of a date object
```sql
extract('isodow' from date) as dow
-- 4 (ISO day of week)
extract('isoyear' from date) as year
-- 2002 (ISO year)
extract('week' from date) as week
-- 52
```

We can combine this with `trunc_date` to extract out sub-parts:
```sql
select extract('year' from date_trunc('decade', date)) as decade
```
