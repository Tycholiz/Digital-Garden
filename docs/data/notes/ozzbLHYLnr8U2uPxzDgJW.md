
## SQL Clause Execution Order
`FROM ➡ WHERE ➡ GROUP BY ➡ HAVING ➡ SELECT ➡ DISTINCT ➡ ORDER BY ➡ LIMIT`

in other words...
- The system first executes the FROM clause i.e. it creates the data set on which the WHERE and SELECT clauses are to be run. This step includes any joins specified in the FROM clause.
- Then, it eliminates rows from this data set by using the filters specified in the WHERE clause.
- It then groups the data set by the columns specified in the GROUP BY clause, and finally runs the SELECT clause. This is why you can never use the aliases specified in the SELECT clause to filter in the WHERE clause - because the aliases haven’t been declared yet.
