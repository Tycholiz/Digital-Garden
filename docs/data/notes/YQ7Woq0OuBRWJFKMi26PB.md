
to connect to a PG databse, there are a few different ways to have users authenticate themselves
- the default authentication method can be found in the `pg_hba.conf` file.
	- therefore, if we want to change the default method from peer authentication to md5 (password), we change it here (remember to restart the service)

## Peer Authentication
- by default, `psql` tries to connect to the postgres database over UNIX sockets. The default authentication method is *peer authentication*, which requires the current UNIX user to have the same username as `psql`
	- spec: Therefore, to connect with peer authentication, we need to be logged in on UNIX as the same username as the postgres username we are trying to connect with
	- ex. if on UNIX we are logged in as user `kyletycholiz`, then simply executing `psql` without arguments will try and log us in as the postgres user `kyletycholiz`. If this user doesn't exist in postgres, then we will get a peer authentication error.
- works by obtaining the client's OS username from the kernel, and using it as the allowed database username.
- Only supported for local connections.
