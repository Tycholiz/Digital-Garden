
`any` compares a scalar value (ie. a base type, like `text` or `number`) to a set of values returned by a subquery.

The form is:
```
expression operator ANY(subquery)
```
In this syntax:
- The subquery must return exactly one column.
- The `ANY` operator must be preceded by one of the following comparison operator =, <=, >, <, > and <>
- The `ANY` operator returns true if any value of the subquery meets the condition, otherwise, it returns false.

Note: `SOME` is a synonym for `ANY`, meaning that you can substitute `SOME` for `ANY` in any SQL statement.
Note: The `= ANY` is equivalent to IN operator.
