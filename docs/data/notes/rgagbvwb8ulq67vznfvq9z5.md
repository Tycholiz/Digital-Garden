
When an object is created, it is assigned an owner (normally the role that executed the creation statement).
- for most objects, the default state is that only the owner (or a superuser) can do anything with the object.

The available privileges are: SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, CONNECT, TEMPORARY, EXECUTE, USAGE, SET, ALTER SYSTEM, and MAINTAIN
- The privileges applicable to a particular object vary depending on the object's type (table, function, etc.).

The owner is the user that has the right to modify or destroy an object

An object can be assigned to a new owner with an ALTER command of the appropriate kind for the object
- ex. `ALTER TABLE table_name OWNER TO new_owner;`

To assign privileges, the GRANT command is used
- ex. `GRANT UPDATE ON accounts TO joe;`

Writing ALL in place of a specific privilege grants all privileges that are relevant for the object type.