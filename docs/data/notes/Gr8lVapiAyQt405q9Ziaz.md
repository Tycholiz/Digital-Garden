
order by the column name.
- SQL doesn't guarantee any ordering of the result set of any query except when you use ORDER BY
- you can use `order by 1` to sort the view or table by 1st column of query's result.

## Implementing kNN (*k* nearest neighbors)
Suppose we want to order a list of cities by their distance to Paris:
- note: `<->` operator is "distance between"
```sql
select name, location, country
    from circuits
order by point(lng,lat) <-> point(2.349014, 48.864716)
   limit 10;
```
