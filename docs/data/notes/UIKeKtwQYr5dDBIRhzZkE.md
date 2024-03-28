
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
