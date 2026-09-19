
# Overview
- When you install an instance of Postgres (ex. in Node), there will be a corresponding Postgres server, since postgres uses a client/server model.
	- if we use different ports, we can have multiple Postgres servers on a single physical server

## Processes
### Postgres Server
- the server program is called `postgres`
- purposes:
	- manages database files
	- accepts connections from clients
	- performs operations on behalf of clients

### Postgres Client
- the client is the application that wants to perform database operations
- clients are diverse in nature, and could be a CLI, GUI, or a library within a web server (like `pg` in node.js)

- Postgres' value is about concurrency and isolation
	- The idea: what happens when 2+ people are trying to do the same thing concurrently? how is that handled?
		- This value is provided by the concept of ACID
		- The fact that we have transactions in postgres means that we are ACID-compliant.
