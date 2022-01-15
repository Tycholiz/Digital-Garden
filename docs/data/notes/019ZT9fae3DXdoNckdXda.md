
# Aggregate Functions
- called aggregate functions because they are functions that act on multiple rows and give us a single output
	- ex. sum, avg, max, min
- used to extract information about whole groups of rows, and allow us to easily ask questions like:
1. What's the most expensive facility to maintain on a monthly basis?
```sql
select max(monthlymaintenance)
from cd.facilities;
```
2. Who has recommended the most new members?
3. How much time has each member spent at our facilities?
4. what is the most popular type of shoe?
- Aggregate functions only work in SELECT or HAVING clauses
Therefore, an aggregate function will take a group of data, perform some function on it, and have a single output that it will print in our result set. An aggregate function can perform transformation on the data, or it can also perform a calculation on it. In either case, the aggregate function is making a consideration of all rows of data, and deciding what to do with it that would result in a single piece of data (int, array etc) that will hold the results of that calculation.
- This is why when we perform aggregate functions, we need to use ORDER BY, because we need to tell the query what we want to apply the aggregate function to.
	- Since the aggregate function results in an aggregate set of data *based on something*, we need to know what that "based on" actually is. Is the data getting combined *based on* age? Is it getting combined *based on* nugget (ie. combining buckets from multiple rows in an effort to make a json array)?
- When you hear "aggregate function", you should think of taking data from multiple rows and combining (likely integers) or transforming (likely objects/arrays) it in some way

[some useful examples](hashrocket.com/blog/posts/faster-json-generation-with-postgresql)

# Pitfalls
- because of the order of execution, aggregate functions cannot be used in the WHERE clause (see queries/WHERE)
	- This restriction exists because the WHERE clause determines which rows will be included in the aggregate calculation; so obviously it has to be evaluated before aggregate functions are computed
