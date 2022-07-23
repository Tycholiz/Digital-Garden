
### Users
- list all users - `SELECT User FROM mysql.user;`
- connect as user "monica" - `mysql -u monica -p`
- change user password - `ALTER USER 'userName'@'localhost' IDENTIFIED BY '<my-password>';`

### Databases
- change database - `use monica;`

### Tables
- show tables - `show tables;`
- drop table - `drop table <table-name>`
- show columns - `show columns from <table-name>`
