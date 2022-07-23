
Views are named queries stored in the database. For queries that we perform often, we can effectively give them an alias. The result set of that query is conceptually placed into a table, which is referencable by the name we give it.
- ex. Imagine we have a join that we perform often. We can put it in a view and get that data like so:
```sql
CREATE VIEW myview AS
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```
- Being liberal with use of Views is a good practice, as they allow us to encapsulate details of the structure of our tables behind consistent inferfaces
	- This value shows itself as the structure of our tables change, yet we continue to have a consistent inferface.
- Besides the read-only views, PostgreSQL supports updatable views.
