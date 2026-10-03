
PostgreSQL includes many SQL tests to validate its query parser, optimizer and executor. It uses a framework named the *regression tests suite*, based on a very simple idea:

1. Run an SQL file containing your tests (using psql)
2. Capture its output to a text file that includes the queries and their results
3. Compare the output with the expected one that is maintained in the repository with the standard diff utility
4. Report any difference as a failure

You can have a look at PostgreSQL repository to see how it’s done, as an example we could pick [src/test/regress/sql/aggregates.sql](https://github.com/postgres/postgres/blob/master/src/test/regress/sql/aggregates.sql) and its matching expected result file [src/test/regress/expected/aggregates.out](https://github.com/postgres/postgres/blob/master/src/test/regress/expected/aggregates.out).
- Implementing regression testing like this is trivial, since the driver is only a thin wrapper around executing standard applications such as *psql* and *diff*

The idea would be to always have a setup and a teardown step in your SQL test files, wherein the setup step builds a database model and fills it with the test data, and the teardown step removes all that test data.

# Tools
[RegreSQL: Regression testings for Postgres](https://github.com/dimitri/regresql)
