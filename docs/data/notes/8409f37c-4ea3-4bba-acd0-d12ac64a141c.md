
# Connecting to Postgres
- When a new user connects to the database, postgres will fork a new process.
	- After this point the client and the new process communicate directly, without intervention from the original process.
	- This shows the dependency on processes rather than threads, giving us more stability at the cost of higher connection start-up costs.
	- each connection into Postgres is going to consume about 10 MB of memory from your database
	- These start-up costs can be overcome with pooling
- In postgres, each client connecting to the database has its own process 
	- connection can be verified by running `select true as "Connection test";`
- *Connection String* - the database url used to connect to the database (`postgres:///...`)
	- if we specify the connection string as `postgres:///mydb`, then we are implicitly passing our computer's username and no password, which would be equivalent to `postgres:///kyletycholiz@localhost:5432/mydb` 
		- spec: are these default variables determined by env variables like PGHOST?
