
`lag` allows us to access access rows that appeared before the current row that we are accessing. We give `lag` an offset and we get back the row that appeared `x` spots before the current one.
- Naturally, this makes `lag` useful in being able to compare the values of the current and previous rows.

The following gives us the previous row.
```sql
lag(invoice, 1)
```
