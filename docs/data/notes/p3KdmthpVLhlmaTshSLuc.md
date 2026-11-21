
The GRANT command has two basic variants: one that grants privileges on a database objects, and one that grants membership in a role.

When you create a new database, any role is allowed to create objects in the public schema. To remove this possibility, you may issue immediately after the database creation:

```sql
REVOKE ALL ON schema public FROM public;
```
after the above command, only a superuser may create new objects inside the public schema, which is not practical. Assuming a non-superuser foo_user should be granted this privilege, this should be done with:

```sql
GRANT ALL ON schema public TO foo_user;
```
