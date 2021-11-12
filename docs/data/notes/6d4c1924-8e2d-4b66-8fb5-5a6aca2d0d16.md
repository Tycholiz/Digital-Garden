
# Assert
`ASSERT` is a convenient shorthand for inserting debugging checks, but is not useful for reporting ordinary messages and errors.

- we can give `ASSERT` a value that is always expects to evaluate to true.
	- if it does, `ASSERT` does nothing further.
	- if it doesn't, an `ASSERT_FAILURE` exception is raised.
- if we don't pass a message, a default "assertion failed" will be used.
```sql
ASSERT v_user is not null, 'user not found, check the users table';
```
