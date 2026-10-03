
A type that holds a single row from a result-set (therefore, not just tables)

`record` type is similar to `row-type`, but `record` doesn't have a predefined structure. The structure only gets established when the `select` or `for` statements assin an actual row to it.

To access a field in the record, you use the dot notation (.) syntax like this:
```sql
record_variable.field_name;
```

a record is not a true data type. It is just a placeholder.
- Also, a record variable can change its structure when you reassign it
