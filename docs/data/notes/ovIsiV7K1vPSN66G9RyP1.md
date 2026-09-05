
Imagine we were in a position where we needed to make complex queries that involved some aggregation functions. What we could do to fulfill this, is create a temporary table, fill it with data (using INSERT), and use a *cursor* to loop through the temporary data, making updates as we go. Finally, we could execute a query against the temporary table, allowing us to obtain the final results. 
- An alternative approach for some data problems is to use *derived tables* (also known as inline views)

A derived table is a virtual table that is created within the scope of a query
- The table is created with a SELECT statement of its own and is given an alias using the AS clause
- the contents of this table can then be used within the query.
- the result is similar to the above naive solution, and doesn't carry the overhead of having to actually CREATE/DELETE tables and run INSERTs on them
	- Therefore, derived tables eliminate the requirement to use cursors and temporary tables

you can think of a derived table as a temporary variable that points to a table that we just defined 

## Using Derived Tables
- Because derived tables are used in place of an actual table, the query to build the derived table goes in the same spot as we normally have our FROM clause. All we need to do is replace the table name with parentheses, and define an alias with AS afterward
- In the below example, all our derived table is really doing is making a new table that we can query against, where instead of having 3 columns: `first_name`, `last_name` and `value`, we have 2 columns: `salesperson` and `value`. This is a contrived example to show that a derived table allows us to transform how the original table appears, which we can then take advantage of to support our more complex queries of the data. 
```sql
SELECT * FROM
(
	SELECT first_name || ' ' || last_name AS salesperson, value_of_sales FROM sales_table
) as derived_sales_table
WHERE value_of_sales > 100 
```
- The data from this inner query can be used in the column list, in joins, and any other area where table data would be acceptable.

In Postgres, we can use the WITH clause to clean up the logic of derived tables:
```sql
SELECT first_name, last_name, age
FROM (
  SELECT first_name, last_name, current_date - date_of_birth age
  FROM author
)
```
becomes
```sql
WITH a AS (
  SELECT first_name, last_name, current_date - date_of_birth age
  FROM author
)
SELECT *
FROM a
WHERE age > 10000
```
- with the above example, if we were anticipating the need to query the data set resulting from that subquery, we could just make it into a view so we wouldn't have to repeat that logic with each query we make

[tutorial](http://www.blackwasp.co.uk/SQLDerivedTables.aspx)
