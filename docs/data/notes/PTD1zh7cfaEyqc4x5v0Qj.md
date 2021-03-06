
# SELECT
- SELECT is known as a "projection" in relational algebra.
	- Once you’ve generated your table reference, filtered it, transformed it, you can step to projecting it to another form
- Within the SELECT clause, you can finally operate on columns, creating complex column expressions as parts of the row.
- SELECT accepts expressions, allowing us to say `select (temp_lo + temp_hi)/2 as temp_avg...`
- The SELECT clause may be one of the most complex clauses in SQL, even if it appears so simple. All other clauses just “pipe” table references from one to another
- In order to understand SQL, it is important to understand everything else first, before trying to tackle SELECT


### SELECT DISTINCT
remove duplicate rows *after* the SELECT has been applied (in other words, after the projection)

### SELECT (inside a WITH)
- purpose is to break down complicated queries into simpler parts

# Combining result set of 2+ SELECT statements
The queries that involve UNION, INTERSECT, or EXCEPT need to follow these rules:
1. The number of columns and their orders must be the same in the two queries.
2. The data types of the respective columns must be compatible.

* * *

### Don't use SELECT *
- By manually listing out each column we want to project, we show the intention of the code. Listing out each column allows for declaring our thinking as developers
- When we implicitly select all columns, if the columns on a table change (eg. we remove a column), then the implementations where we call `SELECT *` may not be happy with the new dataset returned.
	- Maybe the column we just removed was a dependency of that module. If we had manually listed out each column, then the error would have been more clear.
- It isn't efficient to retrieve all bytes of data, when we don't even need them all.
