
A self join allows us to join a table to itself using an inner or outer join
- this allows you to create a result set that joins the rows with the other rows within the same table.

In practice, you typically use a self-join to query hierarchical data or to compare rows within the same table.
- Imagine we need to compare a column of each row with all other rows in that table, and get a result set of only the rows that pass a certain condition
	- ex. we have a table `temperature_readings` that include both the high and low for the day. What we conceptually need to do is take row1 and compare it to every other row. Then we take row2 and compare it to every other row, and so on. Conceptually, this is like a nested for loop, where w1 is the outer loop and w2 is the inner loop:
```sql
select
     w1.city,
     w1.temp_lo as low,
     w1.temp_hi as high,
     w2.city,
     w2.temp_lo as low,
     w2.temp_hi as high
 from weather w1, weather w2
 where w1.temp_lo < w2.temp_lo
 and w1.temp_hi > w2.temp_hi;
```
With self joins, we need to use an alias because you cannot refer to the same table more than once in a query, so we assign it a name when it is joining against itself
- since we are using 2 versions of the same table, we should give both an alias. For example in the below example, give alias `m` for the manager's version and `e` for the employee's version
- [why use aliases?](https://www.reddit.com/r/SQL/comments/9e5yi9/why_use_table_alias_noob_question/)

Two reasons for using a self-join:
1. query parents/child relationship stored in a table
	- ex. in `employee` table, demonstrate relationship between `employee_id` and `reports_to` columns. Here, the employee's `reports_to` would match up with the manager's `employee_id`
		- `INNER JOIN employees m ON m.employeeid = e.reportsto`
2. obtain running totals

