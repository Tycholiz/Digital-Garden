
SQLite is a self-contained C library that keeps the entire database in a single file

- show tables - `.tables`
- show databases - `.databases`
- open database - `.open ~/.config/joplin/database.sqlite`
- show signature of table - `.shema <tablename>`
	- more readable form - `pragma table_info('<tablename>')`

### Migrations
The  package includes a database migrations feature

## Tools
- [Litestream: Fully replicated SQLite](https://litestream.io/)