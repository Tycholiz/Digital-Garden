
# Differences between a column NOT NULL constraint and a CHECK CONSTRAINT not null
a `check constraint` is like a column constraint, except that it's on a table.

Although the end result may be the same, there are some differences:
- check constraints have to be named and belong to the table, while a NOT NULL column is just an option of the latter
- from a performance point of view, creating an explicit not-null constraint is more efficient.
	- only 0.5% performance penalty, so hardly noticeable.
- you have to remove check constraints before removing the associated columns
- a `NOT NULL` is written beside the column name when issuing `\d your_table` on psql, while check constraints are described below a specific session
