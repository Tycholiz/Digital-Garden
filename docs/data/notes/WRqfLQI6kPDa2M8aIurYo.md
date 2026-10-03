
A role is an entity that can own database objects and have database privileges
- can be considered a "user", a "group", or both depending on how it is used
	- in other words, a user role can be part of a group role
- must have CREATEROLE privilege or be a database superuser to create roles.

When a user logs in, we want them to make their queries using a specific role
- any time we are running `CREATE USER` or `CREATE GROUP`, we are running `CREATE ROLE` under the hood.
	- minor difference: `CREATE USER` also logs the user in, so a `role` having the LOGIN attribute can be thought of as a user 
		- the ability to log in just means the role can be input along with a password as part of the connection string: `postgres://admin@localhost/mydb`
	- In the past, there were users and groups. Now, there are just roles. Roles have the ability to log in, have the ability to inherit from other roles, 
		- basically, we moved from the Unix paradigm of users and groups, to the OOP paradigm of having inheritance.
	
Attributes
- attributes define privileges and info for a role 
	- ex. login, perform superuser, database creation, role creation, password, etc
		- by default, roles don't get the ability to log in
- the attributes are listed after the WITH clause, though that word is optional 
```sql
CREATE ROLE kyletycholiz WITH
LOGIN
...
```

Default roles
- the `postgres` user is automatically created when we install Postgres. It is a superuser that we log into postgres with
	- all server processes work on behalf of this user
	- all database files belong to this user
- roles that start with with pg_ are system roles.

- roles are defined at the database cluster level, and so are valid in all databases in the cluster.
cluster → database → schema → table

## Grant
- When an object is created, it is assigned an owner. 
	- The owner is normally the role that executed the creation statement.
	- The right to modify or destroy an object is always the privilege of the owner only
- When we use grant to grant access to a role, the now-empowered role can do 2 things: they can do everything that the source role could do, or they can actually control and become the role that it gained its powers from (this would mean that if an `admin` role became `user_login` role, it would no longer be able to perform any action that have admin-only privileges) 

GRANT has 2 variants:
1. grant privileges on a database object
	- inc. table, column, view, sequence, database, foreign-data wrapper, foreign server, function, procedural language, schema, or tablespace
2. grant membership in a role
- below, we define a role `user_admin`, then give the role `postgres` the ability to do anything that `user_admin` can do.
	- from another viewpoint, `postgres` is gaining the power to do everything that `user_admin` can do.
```sql
CREATE ROLE user_admin;
GRANT user_admin to postgres;
```
- GRANTing on a database doesn't grant rights to the schema within. Similarly, GRANTing on a schema doesnt grant rights to tables within.
	- ex. if we run `grant usage on schema app_public to user_guest`, we grant the role `user_guest` the right to know that the schema exists, but it doesn't give it the right to interact with the tables within.
		- if we want to grant read rights to a table, then we need to run `grant select on table <TABLENAME> to user_guest`
	- If you have rights to SELECT from a table, but not the right to see it in the schema that contains it, then you can't access the table
		- Therefore, the user must first be granted rights to the schema, otherwise the rights on the table are useless. 
		- Likewise, if the user had been granted rights to just the schema, but not the tables within, they would still be locked out. 
			- It's like a directory tree. If you create a directory somedir with file somefile within it then set it so that only your own user can access the directory or the file (mode rwx------ on the dir, mode rw------- on the file) then nobody else can list the directory to see that the file exists. If you were to grant world-read rights on the file (mode rw-r--r--) but not change the directory permissions it'd make no difference. Nobody could see the file in order to read it, because they don't have the rights to list the directory. If you instead set rwx-r-xr-x on the directory, setting it so people can list and traverse the directory but not changing the file permissions, people could list the file but could not read it because they'd have no access to the file. You need to set both permissions for people to actually be able to view the file. 
- the public schema has a default GRANT of all rights to the role public, which every user/group is a member of. So everyone already has usage on that schema.
- opposite of GRANT is REVOKE
*GRANT USAGE* - grants access to objects contained in the specified schema 

## Roles & Auth
- all auth happens through postgres roles and permissions. Postgres is in charge of authenticating requests. 
- Postgres permissions work as a whitelist and not a blacklist (except for functions). 
	- There are very few defaults, and if we want to blow them away entirely, use `alter default privileges`
- when an authenticated user makes a request, the role will be changed for that user, having the effect of restricting queries (can be seen in `current_user` variable)
	- `current_user` is the username of the current execution context 
- Because we have the concept of default roles, we set a default (such as `user_guest` that every user will get by default. When the user demonstrates a verified JWT token, that role gets changed to one with more authorization rights (such as `user_login`).
- JWTs can be generated in postgres with `pgjwt` extension
