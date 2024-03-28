
MySQL is not case sensitive

### Users
- list all users - `SELECT User FROM mysql.user;`
- connect as user "monica" - `mysql -u monica -p`
- change user password - `ALTER USER 'userName'@'localhost' IDENTIFIED BY '<my-password>';`

### Databases
- change database - `use monica;`
- see open transactions - ` SELECT * FROM information_schema.innodb_trx;`

### Tables
- show tables - `show tables;`
- drop table - `drop table <table-name>`
- show columns - `show columns from <table-name>`

### Dump
- Backup: `mysqldump -h <hostname> -u <user> -p <databasename> > dump.sql`
- Restore: `mysql -u root -h 127.0.0.1 -p my-database-name < dump.sql`