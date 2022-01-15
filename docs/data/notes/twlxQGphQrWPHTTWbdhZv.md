
# Object-nature of Postgres
- PostgreSQL is an object-relational database management system (ORDBMS)
- a postgres database itself is an object. It contains other objects, such as tables, views, functions, and indexes. since a database is just an object, multiple postgres databases can be stored inside a single postgres server.
- By making everything an object, Postgres is highly extensible, allowing us to build:
	- data types
	- functions
	- aggregate functions
	- operators
	- index methods
- A special feature of PostgreSQL is table inheritance, meaning that a table (child table) can inherit from another table (parent table) so when you query data from the child table, the data from the parent table also shows up.
