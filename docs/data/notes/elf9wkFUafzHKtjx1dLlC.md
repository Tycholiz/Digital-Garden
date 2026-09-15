
## Data Types
### Composite Type
- a type defined by the "signature" of the row. In other words, the very combination of columns included in a row make up a **composite type**
	- therefore, every column in a given table is an example of a composite type. In fact, every time we create a table, a composite type representing that row's composition is made.
- PostgreSQL allows composite types to be used in many of the same ways that simple types can be used.
	- ex. a column of a table can be declared to be of a composite type.
		- spec: for instance `first_name` and `last_name`?
- In Postgres, columns can be of composite types, meaning that we can have a type defined as (email: string, username: string), and have a column in a table called `identity` and have a column with that type.
- When used in a function, composite types can be thought of as objects, as we would access the value of each subtype with `$1.key`

### Text
- don't use VARCHAR. just use TEXT, and add length limits

#### Citext
- Case insensitive text
- behaves like text, except that it lower-cases everything before comparing it
	- Therefore, we may want to store things like email or postal code in `citext` rather than `text`.

### Serial
- SERIAL is the postgres equivalent of autoincrement in other flavors of SQL
	- Consider using uuid's though.

### Range
- We can specify a range in a single column (ex. timestamps, price etc).
- The real benefit is that you can then have constraints
	- ex. certain time stamps can’t overlap
- To work with ranges, use the [range operators](https://www.postgresql.org/docs/9.3/functions-range.html)
[Demo](https://wiki.postgresql.org/images/7/73/Range-types-pgopen-2012.pdf)
