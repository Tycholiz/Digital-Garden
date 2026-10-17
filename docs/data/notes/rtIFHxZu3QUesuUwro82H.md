
"Take 2+ tables with equal number of columns and combine them into a single result set."
- ie. combines results of 2+ SELECTs into a single result set.
- "append" could have also been an appropriate name

UNION is conceptually opposite of JOIN; it expands the result set in the other direction (y instead of x)
- JOIN is for taking 2 tables and combining their columns, while UNION is for taking 2 tables and combining their rows(ie. records)
```sql
SELECT select_list_1
FROM table1
UNION
SELECT select_list_2
FROM table2
```
- The number and the order of the columns in the select list of both queries must be the same.
- The UNION operator by default removes all duplicate rows from the combined data set (`UNION ALL` will bring them back)
- In practice, you often use the UNION operator to combine data from similar tables, which are not perfectly normalized
- ex. with 2 tables: `popular_movies`, `top_rated_movies`, return the movies found in both tables, removing any duplicates along the way
<!-- TODO fix imagine -->
![8d2bb4d874e9ee042506b53f0a5eb8cd.png](:/202885018e6045e4acd744771788c297)
