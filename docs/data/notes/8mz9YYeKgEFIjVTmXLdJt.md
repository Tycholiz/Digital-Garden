
Like UNION and INTERSECT, EXCEPT returns distinct rows from the first (LHS) query that are not in the output of the second (RHS) query.
- ex. with 2 tables: `popular_movies`, `top_rated_movies`, return the movies that are popular, but not top rated.
	- assuming we have `SELECT * FROM popular_movies` first
<!-- TODO: fix this -->
![daddae8a308e1bf9405e038b101affbd.png](:/d8dc07ccdd01418c816ddaf6951dfdd7)
