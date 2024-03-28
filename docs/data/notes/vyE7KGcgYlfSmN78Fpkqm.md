
## What is it?
A cache of database connections maintained so that the connections can be reused when future requests to the database are required
- after a connection is created, it is placed in the [[pool|general.patterns.creational.pool]] and it is used again so that a new connection does not have to be established

## Why do it?
Connecting a new client to the PostgreSQL server requires a handshake which can take 20-30 milliseconds. During this time passwords are negotiated, SSL may be established, and configuration information is shared with the client & server. Incurring this cost every time we want to execute a query would substantially slow down our application.

The client pool allows you to have a reusable pool of clients you can check out, use, and return
- used to enhance the performance of executing commands on a database
- If you're working on a web application or other software which makes frequent queries you'll want to use a connection pool.

## How does it work?
1. When the connection pool is created by the client (most often at the same time as when the client server starts), it pre-establishes a set number of database connections. These connections sit ready to be used (ie. with status `available`) by clients to the database server.
2. When `.connect()` is called on the database client, one of the connections is borrowed from the pool. The connection that is assigned to the client gets marked as `inUse`
3. After the client finishes the query, it calls `'.close()`, which returns the connection back to the pool (instead of actually closing it), and gets marked as `available`
4. If a client request comes in and there are no available connections, the Pool Manager will either:
    - create a new connection (up to the configured maximum limit)
    - or wait until a connection becomes available.

We can configure the pool to have a minimum number of idle connections, and a maximum number of connections that can possibly be created

Typically, each database client would have its own pool
- ex. If there are 4 Express servers running, then there will be 4 pools
- between those 4 servers and the users sending requests to them sits a *load balancer*

The pool sits between the application server(s) and the database:
> front end client → load balancer → application server → pool → database
