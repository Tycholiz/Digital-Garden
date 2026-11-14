
# Database File
- known by the environment variable PGDATA, this is where all the data for a database cluster is stored.
	- likely `/etc/postgresql/<VERSION>/main/`

### base/
- each directory within `base/` is used by a database node in our cluster. Each directory is named after the database's OID
	- the files inside are the actual data of the relations (ie. tables, indexes, sequences)
- database examples: `postgres`, `template0`, `template1`, `neverforget`, which can be seen from psql with `\l`
	- the `postgres` database is not required to exist. It is simply a default that is connected to if no database is specified in the connection string.
	- `CREATE DATABASE` works by copying the `template1` database (`template0` can be thought of as a more primitive and stripped down version of `template1`

### pg_hba.conf
- a file that controls the authentication of clients to connect to the postgres database
	- HBA stands for host-based authentication.
- after making changes to this file, to put them into effect run `SELECT pg_reload_conf();` or `pg_ctrl reload` with superuser.
- In `psql`, run `SHOW hba_file;`
