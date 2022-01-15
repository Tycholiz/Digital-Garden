
There are generally three methods in PostgreSQL with which you can fill a table with data:
1. Use the `INSERT INTO` command with a grouped set of data to insert new values.
2. Use the `INSERT INTO` command in conjunction with a `SELECT` statement to insert existing values from another table.
3. Use the `COPY` (or `\copy`) command to insert values from a system file.

### RETURNING
- allows us to get back the columns that were successfully inserted into the table
	- ex. INSERT INTO
- Using this method, we can get back default values (like id or timestamps)
- This method is also useful to insert rows into a variable, which we can then simply return from the whole function
```sql
declare
  v_person forum_example.person;
begin
	insert into forum_example.person (first_name, last_name) values
		(first_name, last_name)
		returning * into v_person;
```
