
Subqueries appearing in FROM can be preceded by the key word LATERAL. This allows them to reference columns provided by preceding FROM items. (Without LATERAL, each subquery is evaluated independently and so cannot cross-reference any other FROM item.)

Table functions appearing in FROM can also be preceded by the key word LATERAL, but for functions the key word is optional; the function's arguments can contain references to columns provided by preceding FROM items in any case.

LATERAL JOINs behave more as a correlated subquery, rather than just a plain subquery.
- expressions to the right of a LATERAL join are evaluated once for each row left of it - just like a correlated subquery - while a plain subquery (table expression) is evaluated once only.

For returning more than one column, a LATERAL join is typically simpler, cleaner and faster.
Also, remember that the equivalent of a correlated subquery is `LEFT JOIN LATERAL ... ON true`
