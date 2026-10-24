
A CTE is basically a way to set a computed result set to a variable, which we can then later use (eg. to join against)

a CTE is a temporary result that we can reference with another SELECT.
- A CTE always returns a result set
- They are used to simplify queries
	- ex. you could use one to eliminate a derived table from the main query body.

A good use-case for a CTE is when we imagine that our result set is grouped by date. Imagine that we want to get a result set showing sales by date. We could certainly query a single table and order by date, but what happens when there are no sales on a particular day? We would end up with gaps in our result set. To solve this, we can make a CTE and join our tables to it.
```sql
with v_date as (
	select current_date
)
```

### CTE and UPDATE..FROM
We can use a CTE in combination with `UPDATE..FROM` to update every row in the result set of a different query (the CTE)
- Here, we are updating the `book_orders.purchase_status` column on book_orders that have been `PENDING` for more than 24 hours:
```sql
WITH old_books AS (
	select * from app_public.book_orders as book_orders
	where book_orders.purchase_status = 'PENDING'
	and created_at < NOW() - interval '24 hours'
)
update app_public.book_orders set purchase_status = 'EXPIRED'
from old_books
```

[more](https://www.essentialsql.com/introduction-common-table-expressions-ctes/)
