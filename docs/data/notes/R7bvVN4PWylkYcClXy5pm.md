
When doing an inner join, if either table is unmatched by the other, then those rows will be excluded from the result set.
- Outer joins allow us to select rows from one table that may or may not have the corresponding rows in other tables.
	- a LEFT JOIN returns only the unmatched rows from the left table (the original table we are joining onto)
		- "get me all rows from the left table, even if there are no matching rows in the right table"
	- a RIGHT JOIN returns only the unmatched rows from the right table (the new table we are adding)
- Making a LEFT JOIN and then putting a column from the outer table in the WHERE clause would give us an error, (spec): since that column would be unselectable.

### Example: songs and artists
Imagine we have songs and artists. Each song has 0 or more artists. This is possible because a song might not have an attributed author.

Imagine we query the songs table, then left join artists. This would allow us a result set that includes all the songs without artist. If we had done an inner join, then the songs without an artist would not have ended up in the result set

## Left Outer Join
- called *left outer* because the table on the left of the join operator will have each of its rows in the output at least once, whereas the table on the right will only have those rows output that match some row of the left table.
	- if there is not right-table match, a null value will be substituted in for right-table columns.
	- a common way to distinguish the left and right side of the join is to use aliases
- Left join semantics are to keep the whole result set of the table lexically on the left of the operator, and to fill-in the columns for the table on the right of the left join operator when some data is found that matches the join condition, otherwise using NULL as the column’s value.
- there are times when we do want to include data where there isn't a match (ex. there is no matching FK for a given PK between 2 tables of a corresponding row). In these cases, we have to use an OUTER JOIN.
	- ex. we are doing a left join on "bucket", and we have buckets in our db that have no nuggets. this would result in a `null` value for the "nugget" column associated with that row.
	![2890a1430af9f888455d50224e795554.png](:/2af5c67cf5fb4b898d5323ce1cb0849b)
- It's the LEFT or RIGHT keyword that makes the JOIN an "OUTER" JOIN
	- ie, all JOINs designated either LEFT or RIGHT are by definition OUTER JOINs
	- We use LEFT, RIGHT or FULL to specify which table should have all of its rows returned, regardless of the condition being met.
- the first table is the "left" table
- best practice is to avoid Right Outer Joins
- when joining tables, all rows inclded in the left table will show up on the new table, even if one of the cells is missing, as in prev. example.
- the left outer join starts selecting data from the left table. it compares the 2 values (specified after `ON`).
	- If they are equal, a new row is created that contains columns of both tables and adds the new row to the result set
	- if they are not equal, a new row is still created, but the right table's columns will be filled with `null`
	- If we want, we can add a WHERE clause to this query to only return rows from the left table that do not having matching rows in the right table
		- this effectively cuts out the intersection from the venn diagram
		- ex. `...WHERE tableb_col IS NULL;`
- if we made a venn diagram of table1 and table2, an outer join would give us the outer parts of the venn diagram Union.
	- we will get all of the rows of the left table, but only the "rows in common" that the right table has
		- ex. if the left table has `1`, `2`, `3`, those will always show up no matter what. If the right table has `2`, `3`, `4`, then only `2` and `3` will show up (the rest will be null)
- Venn diagram illustrates the left join that returns rows from the left table that do not have matching rows from the right table:
![6457179c137a03db0dd9765c9cd6c988.png](:/69d6642385194490a8d62f93f8cf1bdb)

## Full Outer Join
- returns a result set that contains all rows from both left and right tables, with the matching rows from both sides if available.
	- In case there is no match, the columns of the table will be filled with NULL.
- similar to the above techniques using `WHERE` clause to cut out the intersection, we can do the same here
	- This would give us the rows from one table that do not have the corresponding rows in another table
![9e04ed55193420bac12aa21a38c81141.png](:/56dbccd24a6d449fa4d6ba8a0427d96a)

### Left vs Right Join
Imagine we have `paymnent_sources`, which has a `payment_method_id` column, and both `coupons` and `credit_cards` has also have a `payment_method_id` column. In other words, a payment_source can reference either a credit_card or a coupon. Let's say we want to query all payment_sources, including all credit_cards and coupons. We would `select from payment_sources`, then `left outer join` the credit_cards table and the coupons table. Therefore, `left join` is the concept of "I have a baseline group of records, and I want to get all attachments to that baseline, without caring about how each attachment relates to other attachements (e.g. credit_cards to coupons)
