
- a collection of databases servers (ie. nodes, instances) that are connected to a single database file. It is made up of one master node, and 1+ slave nodes.
- The benefits of clustering are: data redundancy, load balancing, high availability, and monitoring and automation.
- a collection of databases managed by a single PostgreSQL server instance constitutes a database cluster.
- a cluster is created with the `initdb` command
- in file system terms, a database cluser is a single directory under which all data is stored.
	- normally `/usr/local/pgsql/data` or `/var/lib/pgsql/data` (determined by the package)
- the unix user postgres should own this directory

## Node
- **node** - an instance of a database server
- the master node is typically the only one that gets written to. 
	- The master can operate without slaves, and if there is an emergency, the slave node can be promoted to master, since it has the most up to date data.
- the slave node typically exists as a backup, or as a read replica
	- a read replica means that the read and write traffic can be split between the two
