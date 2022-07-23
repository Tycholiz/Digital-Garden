
When installing new PG extensions, we often need to restart the DB. Extensions need to be registered in `shared_preload_libraries`

## PGXS
We can compile extensions using a tool called `pgxs`
- PGXS is a build infrastructure for extensions
- it allows us to build extensions against an already installed server.

## Misc
- When trying to install an extension, if we get the error message `cannot find "postgres.h"`, this means that to build the extension from source, the postgres header files are needed. There is a package called `postgres-server-dev-<VERSION>`. This will install all header files for us necessary for server development.

## Commands
- show installed extensions - `\dx` OR `SELECT * FROM pg_extension`
- show available extensions - `SELECT * FROM pg_available_extensions`

# Notable Extensions
### pg_stat_statements
Allows us to achieve a general system-wide analysis to help us with things like indexing strategy.

It’s possible to have a list of the most common queries in terms of number of times the query is executed, and the cumulative time it took to execute the query.
