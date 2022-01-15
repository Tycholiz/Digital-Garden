
# Schema
A schema is a logical namespace that can contain database objects such as tables, views, indexes, data types, functions, stored procedures and operators
- by default, we use the `public` schema, if none is specified
- a schema holds all objects, except for roles and tablespaces.
A schema is essentially a namespace: it contains named objects (tables, data types, functions, and operators) whose names can duplicate those of other objects existing in other schemas. Named objects are accessed either by "qualifying" their names with the schema name as a prefix, or by setting a search path that includes the desired schema(s). A CREATE command specifying an unqualified object name creates the object in the current schema (the one at the front of the search path, which can be determined with the function current_schema).
- "a schema is a namespace that contains named database objects such as tables, views, indexes, data types, functions, stored procedures and operators."
- A database can contain one or multiple schemas and each schema belongs to only one database.
- ex. you may have sales schema that has staff table and the public schema which also has the staff table. When you refer to the staff table you must qualify it as follows:
	- public.staff
	Or
	- sales.staff
- you can use one schema for the tables that will be exposed to GraphQL, another for the tables that should be completely private (e.g. where you store the bcrypted user passwords or other secrets never to be exposed!)

## Good practice for Schema building:
**app_public** - tables and functions to be exposed to GraphQL (or any other system) - it's your public interface. This is the main part of your database.
**app_hidden** - same privileges as `app_public`, but it's not intended to be exposed publicly. It's like "implementation details" of your app_public schema. You may not need it often.
**app_private** - SUPER SECRET STUFF 🕵️ No-one should be able to read this without a SECURITY DEFINER function letting them selectively do things. This is where you store passwords (bcrypted), access tokens (hopefully encrypted), etc. It should be impossible (thanks to RBAC (GRANT/REVOKE)) for web users to access this.
- security definer effectively changes the role for the function being executed

#### "User" tables
- we should have 2 different tables about users. One should be on the private schema (`user_account`), where password and email will be kept. The other should be on the public schema, where everything else about the user is kept. The two tables have a 1:1 relationship, where the `private.user_account` table has a reference to the `public.user` table.
	- [good example](https://github.com/dijam/graphile-jwt-example/blob/master/db.sql)

* * *

According to the SQL standard, the owner of a schema always owns all objects within it. PostgreSQL allows schemas to contain objects owned by users other than the schema owner. This can happen only if the schema owner grants the CREATE privilege on their schema to someone else, or a superuser chooses to create objects in it.
