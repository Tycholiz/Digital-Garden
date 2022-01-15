
All psql commands are just convenience wrappers around underlying SQL statements that are being executed
- ex. `\l+` executes an sql query on the `pg_catalog.pg_database` table, joining the `pg_catalog.pg_tablespace` table

## Connecting to psql (from shell)
Either:
```
psql -d mydb -h localhost -p 5432 -U kyle
```
or simply:
```
psql -d postgresql://kyle@localhost:5432/mydb
```
note: `@` symbol must be entered as `%40` to properly interpret it.
- ex. if hostname=`kyle@db1464.azure.com`, I connect like `psql -d postgresql://kyle%40db1464.azure.com`

# CLI (psql/pgcli)
show help for a command - `\h <sql command>`

connect to db - `\c mydb`
show schemas - `\dn`
display tables in "public" schema - `\dt public.*`
display views in "public" schema - `\dv public.*`
display table signature `\d <TABLENAME>`
display types `\dT <SCHEMA>`
display enum values ` \dT+ <TYPE>`
display roles - `\du`
display schema - `\dn`
display functions - `\df`
display function definition - `\ef app_public.register_user`
display extensions - `\dx`
read in commands from a sql file- `\i`
- ex. `\i migration.sql`
- this is an alternative to shell-level `psql -h localhost -p 5432 -U kyletycholiz -d f1db < migration.sql`
set variable in psql - `\set DATABASE_OWNER nf_dev`
set variable's value from command line - `psql --variable "DATABASE_OWNER=nf_dev"`
list all databases, and show their respective size on disk - `\l+`

display info about current connection - `\conninfo`
- current user, database, port etc.

cycle command history - `<C-r>`

## Named Queries
Create a new named query
```
\ns <name> select * from ...
```

Call named query
```
\n <name>
```

Delete a named query
```
\nd <name>
```

### Positional Parameters
Named queries support shell-style parameter substitution. Save your named query with parameters as placeholders (e.g. $1, $2, $3, etc.):

```
\ns user_by_name select * from users where name = '$1'
```
When you call a named query with parameters, just add the parameters after the query's name. You can put quotes around arguments that include spaces.

```
\n user_by_name "Skelly McDermott"
```
