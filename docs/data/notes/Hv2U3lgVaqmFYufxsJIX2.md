
# Database Users
- each database cluster has a set of database users, which are distinct from the users that the OS of the server manages.
- database users are global across a cluster installation.
- users own database objects, such as tables. These owners can assign priveleges on those objects to other users.
- freshly initialized postgres systems will always contain a predefined user with ID 1, which has the same name as the OS user that initialized the db cluster. However, it is often a best practice to name this user `postgres` instead.
	- this is a superuser
	- To create more users, we need to connect as this user first.
