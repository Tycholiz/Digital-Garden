
PL/pgSQL is an interpreted language, which means that the function body is not evaluated for syntactical soundness as in compiled programming languages like Pascal or C++. Therefore you can write a function which only reveals any errors when it is invoked.

Think of sql commands as having the potential to return rows. In this sense, they are like functions.
- in javascript we can do something like this:
```js
const returnedValueFromFn = fn()
```

- in plpgsql we can do something like this:
```sql
select * from users returning * into v_users;
```

Since these sql commands are like functions, we can imagine that `v_user_id` is an argument to the following query:
```sql
select * from users where id = v_user_id;
```
- When executing a SQL command in this way, PL/pgSQL may cache and re-use the execution plan for the command

# E Resources
[Return multiple fields without making new type](https://stackoverflow.com/questions/4547672/return-multiple-fields-as-a-record-in-postgresql-with-pl-pgsql)
