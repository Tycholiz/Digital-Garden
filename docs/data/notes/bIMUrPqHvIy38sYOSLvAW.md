
#### Delete using join
`DELETE JOIN` isn't supported, but we can imitate the functionality:
```sql
DELETE FROM table_name1
USING table_expression -- the table(s) we are "joining"
WHERE condition
RETURNING returning_columns;
```
