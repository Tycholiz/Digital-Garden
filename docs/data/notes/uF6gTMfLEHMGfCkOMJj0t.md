The system catalogs are just regular tables that are updated automatically in response to events.
- ex. the `pg_database` catalog is updated in response to `CREATE DATABASE MY_DB`

The system catalogs are the place where a RDBMS stores:
- schema metadata, such as information about tables and columns
- and internal bookkeeping information

System catalogs are _not_ part of the SQL standard, and are core to Postgres, unlike the [[information schema|pg.system.information-schema]], which is SQL-compliant

