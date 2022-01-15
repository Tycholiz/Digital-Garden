
A view is like a derived table, but instead of it only living within the context of a single query, it exists more in perpetuity
- in other words, it is the result set of a stored query
- users of the database can query the view just as they would query any other table

While a view is a virtualized table that represents the result of a particular db query, when a query is made against a view, that query is converted into a query that can be made against the base table (in other words, the underlying base table(s) that the view is based off of)
