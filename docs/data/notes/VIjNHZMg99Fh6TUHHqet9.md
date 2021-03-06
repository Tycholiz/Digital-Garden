
### Alter column to have unique constraint
`ALTER TABLE foo ADD UNIQUE (column_name);`

### add/drop column constraint
```sql
alter table invoice_items alter column invoice_id set not null;
alter table invoice_items alter column invoice_id drop not null;
```

### Add non-nullable column to a table
This can be tricky, since running
```sql
alter table invoice_items add column payment_id uuid not null ...
```

will result in an error, since we are trying to modify existing rows in a table to have a `null` value for the new column

Instead, we have a bit of a workaround, setting some junk default value to get around the constraint.
```sql
alter table mytable add column mycolumn character varying(50) not null default 'foo';
-- ... some work (set real values as you want)...
alter table mytable alter column mycolumn drop default;
```

In the case where we are adding a non-nullable FK, we can take a bit of a different approach:
```sql
alter table invoice_items add column if not exists payment_id uuid references payments;
-- ... some work (set real values as you want)...
alter table invoice_items alter column payment_id set not null;
```

#### Alternative approach: check constraint
[[Differences between a column NOT NULL constraint and a CHECK CONSTRAINT not null|dendron://code/sql.constraint#differences-between-a-column-not-null-constraint-and-a-check-constraint-not-null]]

The trick here is that you can issue a `NOT VALID` option when adding a check constraint. This will tell PostgreSQL that you are aware that the constraint may not be valid on existing data and that it does not have to check it. Subsequent inserts or updates, however, will be enforced.
```sql
alter table invoice_items add constraint payment_id_not_null check (payment_id is not null) not valid;
```
After you have done your operations, then we can `validate` the sql:
```sql
alter table invoice_items validate constraint payment_id_not_null;
```
This will scan the table and ensure that the constraints are all passing
