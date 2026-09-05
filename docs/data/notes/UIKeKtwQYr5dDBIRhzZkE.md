
we can update a table while leveraging the commands that help us create a result set.
- ex. imagine we discover that all temperature readings after Nov 14 are off by 2 degrees:
```sql
UPDATE weather
SET temp = temp - 2
WHERE date > '2020-11-14'
```
- UPDATE always requires a SET token.

[[pg.query.CTE]]

### Joining
```sql
update app_public.payments as payments
  set stripe_pi_code = book_orders.stripe_pi_code
  from app_public.book_orders as book_orders
  where payments.book_order_id = book_orders.id;
```

### UPDATE..IN
If we need to update multiple rows and the data to be updated is the same for all columns, we can use `UPDATE..IN`
- ex. if we need to set a status column to `expired` for a list of records, we only need to get an array of the ids and run the query on it:

```sql
update invitations set status = 'expired' where id in (1, 2, 3);
```