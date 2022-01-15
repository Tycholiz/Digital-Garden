
# Connection Pool
- a cache of database connections maintained so that the connections can be reused when future requests to the database are required
	- after a connection is created, it is placed in the pool and it is used again so that a new connection does not have to be established
- The client pool allows you to have a reusable pool of clients you can check out, use, and return
	- used to enhance the performance of executing commands on a database
- If you're working on a web application or other software which makes frequent queries you'll want to use a connection pool.
- Connecting a new client to the PostgreSQL server requires a handshake which can take 20-30 milliseconds. During this time passwords are negotiated, SSL may be established, and configuration information is shared with the client & server. Incurring this cost every time we want to execute a query would substantially slow down our application.
- Typically, each application server instance would have its own pool
	- ex. If there are 4 Express servers running, then there will be 4 pools
	- between those 4 servers and the users sending requests to them sits a *load balancer*
- The pool sits between the application server(s) and the database:
```
CLIENTs → load balancer → Express → pool → database
```

With Postgraphile
- The rootPgPool is not normally what is used with PostGraphile because its privileges are too elevatedt. typically you use rootPgPool for authentication tasks like Passport.js.
