
### Why?
- Connecting a new client to the PostgreSQL server requires a handshake which can take 20-30 milliseconds. During this time passwords are negotiated, SSL may be established, and configuration information is shared with the client & server. Incurring this cost every time we want to execute a query would substantially slow down our application.

- The PostgreSQL server can only handle a limited number of clients at a time. Depending on the available memory of your PostgreSQL server you may even crash the server if you connect an unbounded number of clients. 

- PostgreSQL can only process one query at a time on a single connected client in a first-in first-out manner. If your multi-tenant web application is using only a single connected client all queries among all simultaneous requests will be pipelined and executed serially, one after the other. No good!

The client pool allows you to have a reusable pool of clients you can check out, use, and return. You generally want a limited number of these in your application and usually just 1. Creating an unbounded number of pools defeats the purpose of pooling at all.

### Implementing
You must always return the client to the pool if you successfully check it out, regardless of whether or not there was an error with the queries you ran on the client. If you don't check in the client your application will leak them and eventually your pool will be empty forever and all future requests to check out a client from the pool will wait forever.

### Connection Pooling
- a pool sits between the postgres frontend (ex. postgraphile, postgres-node) and the postgres server. The pooler speaks the postgres language, so understands all incoming queries.
	- Therefore, the server sees the requests as coming from the pg pool, and the postgres frontend sees the pool as handling the requests. 
		- Therefore,
			- without a pool, if we had 20 db connections and 8 were idle, all 20 would be eating up postgres resources (memory)
			- with a pool, those 8 idle connections remain in the pool and don't connect to the database, freeing up resources for us. From the database perspective, there are only 12 connections.
	- the frontend/backend paradigm here is no different from in web developent. In postgres, we have users who interact with the database. The client generates a query, and that query gets sent off to the postgres backend, where it interprets the request, performs the actions, then returns the data to the frontend. 
- when a client is connected to the pool, we say it is a "virtual connection", and if the pooled client is connected to the database, it is a "physical connection" 
- 2 main options: `pgpool-II` and `pgBouncer`
	- pgBouncer does one job and does it well. pgpool-II is more of a swiss army knife, with load balancing included
		- `pgpool-II` seems to be more out-dated
	- [comparison](https://scalegrid.io/blog/postgresql-connection-pooling-part-4-pgbouncer-vs-pgpool/)

#### pgBouncer
- When PgBouncer receives a client connection, it first performs authentication on behalf of the PostgreSQL server.
	- when a password is provided, one of 2 things happens:
		1. PgBouncer first checks the userslist.txt file – this file specifies a set of (username, md5 encrypted passwords) tuples. If the username exists in this file, the password is matched against the given value. No connection to PostgreSQL server is made. 
		2. If passthrough authentication is setup, and the user is not found in the userslist.txt file, PgBouncer searches for an auth_query. It connects to PostgreSQL as a predefined user (whose password must be present in the userslist.txt file) and executes the auth-query to find the user’s password and matches it to the provided value.
- When the client executes an SQL statement:
	1. PgBouncer checks for a cached connection
	2. if found, it returns the connection to the client. Otherwise, it creates a new connection
![a891779b4493983dd662f11960423a72.png](:/bde888d39a1c41fa99e619df6db6f56b)
