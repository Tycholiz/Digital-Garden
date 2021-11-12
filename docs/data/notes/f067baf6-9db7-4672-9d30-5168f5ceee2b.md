
The main hypothesis is that we should be able to prevent access to specific rows of data based on a policy. That means our application logic only has to worry about `SELECT * FROM my_table` and RLS will handle the `WHERE user_id = my_user_id` part automagically.
- To put it another way: our queries should only contain the clauses requested by our interfaces and not the filters and conditions demanded by access control in a multi-tenant data store.
- The current PG role that is accessing the table must have been `grant`ed permission to use it. Otherwise, RLS errors will arise when we try to alter something in the table as that role, because we won't be able to access the table from the outset.

a function marked as `SECURITY DEFINER` will bypass RLS

# Row Level Security (RLS)
- While GRANT is the privilege system of Postgres, Tables can also have Row Security policies that restrict (on a per-user basis) which rows can be interacted with (INSERT, UPDATE, DELETE, SELECT)
	- Policies are created using the CREATE POLICY command
	- spec: `grant` means "I am giving *this* user the privilege to use *this* table". `create policy` means "I am specifying the requirements of any particular row that must be satisfied before it is accessed by user"
- The basis of row level security is to create policies that define how a user interacts with rows within a given table.
- By default, tables do not have any policies, and RLS must be opted-in for each table.
	- When RLS is enabled, all rows are by default not visible to any roles (superusers still have access)
- If the value in parentheses after USING evaluates to true, then the user gets permission
- ex. imagine we have a chat app, and we want to ensure a user can only see messages sent by him, and messages intended for him. Also, we want to ensure that users cannot modify the `message_from` column to make it seem that the message is coming from someone else:
```sql
CREATE TABLE chat (
  message_uuid    UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  message_time    TIMESTAMP NOT NULL DEFAULT now(),
  message_from    NAME      NOT NULL DEFAULT current_user,
  message_to      NAME      NOT NULL,
  message_subject VARCHAR(64) NOT NULL,
  message_body    TEXT
);

CREATE POLICY chat_policy ON chat
  USING ((message_to = current_user)
  OR (message_from = current_user))
  WITH CHECK (message_from = current_user)
```
- RLS can be implemented using jwt claims to verify that the user is who they say they are, if we do not want to use `current_user`:
	- the second arg true means "return null if the setting is missing"
```sql
CREATE POLICY chat_policy ON chat
  USING ((message_to = current_setting('request.jwt.claim.email', true))
  OR (message_from = current_setting('request.jwt.claim.email', true)))
  WITH CHECK (message_from = current_setting('request.jwt.claim.email', true))
```
- When request contains a valid JWT with a role claim (`jwt.claims.role`), we should switch to the role with that name for the duration of the HTTP request

### RLS Policy using external tables
- What if we want to enable RLS where `user_id = current_user_id()`, but the current table does not keep a `user_id` column?
- If we are adding an RLS policy to T1, but the policy depends on a `JOIN`able table T2, then T2 must have `grant`ed privileges to the PG role accessing the table.

```sql
create policy t2_policy_update on t2 for update using (
  exists (
    select 1
      from t1
      inner join t0
        on t1.t0id = t0.id
      where t0.u = session_user
        and t1id = t1.id
  )
)
```
Subqueries in RLS policies respect the RLS policies of the tables they reference

[source](https://stackoverflow.com/questions/41354818/postgresql-row-level-security-involving-a-view-or-a-select-with-join)

## Per-command Policies
### UPDATE
- Since `UPDATE` involves pulling an existing record and replacing it with a new modified record, `UPDATE` policies accept both a `USING` expression and a `WITH CHECK` expression
  - `USING` determines which records the `UPDATE` command will see to operate against
  - `WITH CHECK` defines which modified rows are allowed to be stored back into the table.
    - If the updated value fails the `WITH CHECK` expression, there will be an error.
    - If only a `USING` clause is specified, then it will be used for both `USING` and `WITH CHECK` cases (ie. `WITH CHECK` is implemented for us implicitly)
- Typically an `UPDATE` command needs to read data from columns in the relation being updated (e.g. in a `WHERE` clause, or `RETURNING` clause, or right side of a `SET` clause).
  - In cases such as these, `SELECT` rights are required on the relation being updated, in addition to the `UPDATE` right.

* * *

## Anatomy
### WITH CHECK vs USING
- USING is applied before any operation occurs to the table’s rows
	- ex. in the case of updating a nugget, one could not update a row that does not have the appropriate user_id in the first place
	- must use USING with DELETE commands because a delete changes no rows, and only removes current ones.
	- USING implicitly runs a WITH CHECK with the same clause that USING received, meaning that the verification operation runs both before and after the data is inserted.
- WITH CHECK is run after an operation is applied, so if it fails, the operation will be rejected
	- ex. in the case of an insert, Postgres sets all of the columns as specified and then compares against WITH CHECK on the new row
	- must use WITH CHECK with INSERT commands because there are no rows to compare against before insertion

### Permissive or Restrictive
RLS policies can be either permissive or restrictive
- permissive (default) - in consideration of all RLS policies, only 1 must pass
- restrictive - in consideration of all RLS policies, all must
```
create policy select_all on table_name as permissive using (true)
```

* * *

### Infinite Recursion
Imagine we are making a RLS policy for `select` on a given table. If we then try and `select` that same table within the `using()` function, we will get an infinite recursion as a result.

* * *

### Check if RLS enabled
```sql
select relname, relrowsecurity, relforcerowsecurity
from pg_class
where oid = 'your_table_name_with_schema'::regclass;
```
or
```sql
select * from pg_tables where tablename = 'your_table_name_without_schema'
```

# UE Resources
[Good information about RLS](https://info.crunchydata.com/blog/a-postgresql-row-level-security-primer-creating-large-policies)
[RLS using columns from other tables](https://medium.com/@ethanresnick/there-are-a-few-faster-ways-that-i-know-of-to-handle-the-third-case-with-rls-9d22eaa890e5)
[](https://blog.crunchydata.com/blog/a-postgresql-row-level-security-primer-creating-large-policies)
