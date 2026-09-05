
The WHERE clause allows you to filter rows based on a specified condition. However, the HAVING clause allows you to filter groups of rows according to a specified condition.
- In other words, the WHERE clause is applied to rows while the HAVING clause is applied to groups of rows
- The WHERE clause applies the condition to individual rows before the rows are summarized into groups by the GROUP BY clause. However, the HAVING clause applies the condition to the groups after the rows are grouped into groups.
ex. from a People table, we can SELECT income, and "filter" in the rows that have income > 1000000: `... HAVING income > 1000000;`
In contrast with WHERE, HAVING selects group rows after groups and aggregates are computed. Thus, the WHERE clause must not contain aggregate functions; it makes no sense to try to use an aggregate to determine which rows will be inputs to the aggregates.
- The HAVING clause almost always contains aggregate functions.
