
### count(*)
Counts the number of rows returned by a `select *` statement

### count(colname)
Counts the number of rows, but does not consider `null` values of the specified column

### count(DISTINCT colname)
Counts the number of rows, but does not consider `null` values of the specified column, and does not consier duplicate values
- ex. if we are selecting from a `users` table and there is a column `role`, we can `select count(distinct role)...` and we will get a small list of role possibilities

We often use the COUNT() function with the GROUP BY clause to return the number of items for each group. For example, we can use the COUNT() with the GROUP BY clause to return the number of films in each film category.
