
A function is a reusable block of SQL code that returns a scalar value of a set of rows.
- Functions are transactional by nature. If there is an error somewhere in the function, then the function will be rolled back.
	- If a function is called within a transaction block, and the executing code does not reach the concluding `COMMIT`, then all code executed within the function will roll back as well.
	- Any `BEGIN...EXCEPT` blocks within the function operate like savepoints like the `SAVEPOINT` and `ROLLBACK TO <SAVEPOINT>` SQL statements.
- SQL functions execute an arbitrary list of SQL statements, returning the result of the last query in the list.
	- In the simple (non-set) case, the first row of the last query's result will be returned
	- Alternatively, an SQL function can be declared to return a set, which allows *all* rows of the last query's result to be returned
- allows us to write functions that can interact on the database. For instance, we can create a function that combines `first_name` and `last_name` to give us `fullName`
- Since PostgreSQL permits function overloading, the function name alone does not uniquely identify the function to be called;
	- the parser must select the rigt function based on the data types of the supplied arguments.
- Functions and operators in PostgreSQL support polymorphism and almost every part of the system can be extended.
- By default, functions can be executable by public
- anonymous IIFEs can be invoked anywhere in the SQL by escaping the function identifiers:
```sql
DO \$\$
BEGIN
  EXECUTE 'GRANT appname TO ' || user;
  EXECUTE 'GRANT appname_authenticator TO ' || user;
END;
\$\$ LANGUAGE PLPGSQL;
```

## Using functions in queries
- say we have a function that returns a composite type:
```sql
CREATE FUNCTION new_emp() RETURNS emp AS $$
    SELECT ROW('None', 1000, 25, 'yoyo')::emp;
$$ LANGUAGE SQL;
```
- here we are returning a single column that is a composite type of the signature: string, int, int, string. We also coerce it to the composite type related to the whole `emp` table. This means that the `emp` table has all 4 of those rows, and the act of us coercing means the output of the function can be used anywhere that an explicit `emp` type is required.
- we could make a SELECT statement to get back a one-column table by using `SELECT new_emp()`
- since the composite type is also a sort of virtual table, we can use the function as a "table function": `SELECT * FROM new_emp();`, which will return 4 columns.

## Syntax
- use `$$` to open and close the function:
- stable means that this function does not mutate the database

Terms
- `setof` - Sets emulate rows of tables.
	- `returns setof` and `returns table(column)` are equivalent
	- When an SQL function is declared as returning SETOF sometype, the function's final query is executed to completion, and each row it outputs is returned as an element of the result set.
		- This feature is normally used when calling the function in the FROM clause, since everything after FROM would get interpreted as if it were a base table. In other words, we can add some columns to a base table, and query it as if those added rows were permanent
			- ex. imagine having a function that determined if your salary was above $100,000. we can use a function to get the boolean result and attach it to the base table, so that we can use `FROM employee, getAboveHundred(emp1)
```sql
CREATE FUNCTION add(a int, b int) returns int as $$
 select a + b
$$ language sql stable;
```

# PL/PGSQL
### Declare
- Declare a variable of specified type

### Function Declarations
#### Strict
- this means that if the function receives null input, then the function won't be called and the output will automatically be null as well.
	- ex. imagine we have a `register_user` function, that takes name, email and password as inputs. If the function does not receive `name`, we want it to fail.

#### Security Definer/Invoker
- Security Definer - "the privileges of the function use the privileges (security clearance) of the definer"
- Security invoker - "use privileges (security clearance) of the user who invoked the function
- this means that the function is created with the privileges of the pg role that created it.
- This is a way to heighten the rights of the function, and can be thought of similar to sudo, instead of rights being elevated to superuser, rights are only elevated to the creator of the function itself
	- ie. if we run our migrations with user `postgres`, then using `security definer` will give our function the rights of `postgres`. This is helpful if we want to insert into a table that is part of a very strict schema, such as `app_private`
- we can use `security definer` to by pass RLS

### UE Resource
[insert returning into variable](https://www.xspdf.com/resolution/50004699.html)
