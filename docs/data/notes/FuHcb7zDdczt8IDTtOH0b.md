
## What is it?
To partition is to divide data across tables or databases stored on multiple nodes.

## Why do it?
The justification for partitioning a database is that, after a certain scale point, it is cheaper and more feasible to scale horizontally by adding more machines than to grow it vertically by adding beefier servers.

There are two challenges when we try to distribute data:
1. How do we know on which node a particular piece of data will be stored?
2. When we add or remove nodes, how do we know what data will be moved from existing nodes to the new nodes? Additionally, how can we minimize data movement when nodes join or leave?

Partitioning is usually combined with [[replication|db.distributed.replication]] so that copies of each partition are stored on multiple nodes

By design, every partition operates mostly independently, which is what allows a partitioned database to scale to multiple machines. 

The amount of partitions you have should be at least double the number of instances.
- This allows for growth; if you need to add instances then you can move one partition to another instance.

## Types of Partitioning
There are many different schemes one could use to decide how to break an application database into multiple smaller DBs. Three of the most popular schemes used by various large-scale applications are horizontal partitioning, vertical partitioning, and directory-based partitioning.

### Horizontal Partitioning (a.k.a. Range-based partitioning)
In this scheme, we put different rows into different tables. 
- ex. if we store different places in a table, we can decide that locations with ZIP codes less than 10000 are stored in one table and places with ZIP codes greater than 10000 are stored in a separate table.
    - The key problem with this approach is that if the value whose range is used for Partitioning isn’t chosen carefully, then the partitioning scheme will lead to unbalanced servers. In the previous example, splitting locations based on their zip codes assume that places will be evenly distributed across the different zip codes. This assumption is not valid as there will be a lot of places in a thickly populated area like Manhattan as compared to its suburb cities.

Consider the flexibility afforded when you take a horizontal partitioning strategy. We can have a large number of logical partitions to accommodate future data growth, such that in the beginning, multiple logical partitions reside on a single physical database server. Since each database server can have multiple database instances running on it, we can have separate databases for each logical partition on any server. So whenever we feel that a particular database server has a lot of data, we can migrate some logical partitions from it to another server. We can maintain a config file (or a separate database) that can map our logical partitions to database servers; this will enable us to move partitions around easily. Whenever we want to move a partition, we only have to update the config file to announce the change.

See [[sharding|db.distributed.partitioning.sharding]], a specific type of horizontal partitioning

### Vertical Partitioning
In this scheme, we divide our data to exist on different servers based on their domain.
- ex, if we are building an Instagram-like application where we need to store data related to users, photos they upload, and people they follow, we can decide to place user profile information on one DB server, friend lists on another, and photos on a third server etc.

Vertical Partitioning is straightforward to implement and has a low impact on the application. The main problem with this approach is that if our application experiences additional growth, then it may be necessary to further partition a domain specific DB across various servers (e.g. it would not be possible for a single server to handle all the metadata queries for 10 billion photos by 140 million users).

### Directory-Based Partitioning
A loosely coupled approach to work around issues mentioned in the above schemes is to create a lookup service that knows your current partitioning scheme and abstracts it away from the DB access code. So, to find out where a particular data entity resides, we query the directory server that holds the mapping between each tuple key to its DB server. This loosely coupled approach means we can perform tasks like adding servers to the DB pool or changing our partitioning scheme without having an impact on the application.

## Problems that arise from partitioning a database
Most of these constraints are due to the fact that operations across multiple tables or multiple rows in the same table will no longer run on the same server.

1. *Joins and Denormalization*: Performing joins on a database that is running on one server is straightforward, but once a database is partitioned and spread across multiple machines it is often not feasible to perform joins that span database partitions. Such joins will not be performance efficient since data has to be compiled from multiple servers. 
- A common workaround for this problem is to denormalize the database so that queries that previously required joins can be performed from a single table. Of course, the service now has to deal with denormalization’s perils, such as data inconsistency.

2. *Referential integrity*: just like how performing a cross-partition query on a partitioned database is not feasible, trying to enforce data integrity constraints such as foreign keys in a partitioned database can be extremely difficult.
- Most RDBMS do not support foreign keys constraints across databases on different database servers. This means, applications that require referential integrity on partitioned databases often have to enforce it in application code. Often in such cases, applications have to run regular SQL jobs to clean up dangling references.

3. *Rebalancing*: There could be many reasons we have to change our partitioning scheme:
    - The data distribution is not uniform, e.g., there are a lot of places for a particular ZIP code that cannot fit into one database partition.
    - There is a lot of load on a partition, e.g., there are too many requests being handled by the DB partition dedicated to user photos.

In such cases, either we have to create more DB partitions or have to rebalance existing partitions, which means the partitioning scheme changed and all existing data moved to new locations. Doing this without incurring downtime is extremely difficult. Using a scheme like directory-based Partitioning does make rebalancing a more palatable experience at the cost of increasing the complexity of the system and creating a new single point of failure (i.e. the lookup service/database).

## Partitioning Strategies
- *Hot spot* - a partition disproportionately high load.
- *skew* - the phenomenon of some partitions having more queries made to them, or some partitions holding more data.

### by Key Range
Imagine we had a database of the English dictionary. Our partitioning strategy could be "have 2 letters per partition" (A+B in partition1, C+D in partition2, and so on). This gives us the benefit of knowing exactly which partition to search.
- naturally, this 2 letter per partition approach would lead to some partitions being much bigger than others, so the boundaries would need to adapt to the data.

Within each partition, we can alphabetize the keys making range scans easy.

The downside of Key-range partitioning is that certain access patterns can lead to hot spots.
- ex. imagine we have an IoT device that writes many logs of information, keyed by a timestamp. Therefore, our strategy is to partition by date. If we know have a month, we know exactly which partition the data is in. The downside is that we have all writes going in a single partition, while the rest sit idle. To remedy this problem, we could prepend each timestamp with the name of the sensor, in the hopes that writes get more evenly spread across all partitions.

### by Hash of Key
With this method, we assign a range of hashes to each partition. When writing to the database, we simply hash the key, and insert it into whichever partition would be responsible for it.
- Because a [[hash|crypt.hashing]] is generated from the key, we get a uniform distribution of partitions with this method.

The downside of this strategy is that we can no longer efficiently query ranges. While before we could potentially limit our range query to a single partition, now we need to involve all partitions.
- Cassandra uses a hybrid approach, whereby a table is declared with a compound primary key consisting of several columns. Only the first part of that key is hashed, which determines which partition it will belong to. The other columns are used as a concatenated index for sorting the data in Cassandra’s SSTables. As a result, while we cannot perform a range query over the first column, the rest can be range queried.
    - ex. consider a social media site where users can make posts. In a one-to-many relationship such as this, where one user can make multiple posts, we can define the compound primary key as (`user_id`, `update_timestamp`), and make an efficient range query on the timestamp.

While this strategy heavily reduces hotspots, it does not eliminate them. 
- ex. imagine user profiles of Twitter being stored across many partitions. Because some users are more popular than others, it's reasonable to expect a skew in querying for certain data. Imagine some global event occurs which causes everyone to interact with Donald Trump's user. People retweeting or commenting on Donald Trump's tweets will result in a large volume of writes to the same key (where the key is perhaps Donald's userId, or the ID of his tweet)
    - In cases such as this, if one key is known to be very hot, a simple technique is to add a random number to the beginning or end of the key. Just a two-digit decimal random number would split the writes to the key evenly across 100 different keys, allowing those keys to be distributed to different partitions.

#### CRC-32 Hashing
CRC-32 is a hashing function. Since the output is in hexadecimal, it can be represented as a decimal number, and can have calculations done on it.

When we pass an id to CRC-32, we get back a hexadecimal value:
$$
CRC32("id-123") = F9FDB2C9 (hex) 
\\
or
\\
4194153161 (decimal)
$$
Now we can use that number to determine which of 6 partitions to live on by using modulus:
$$
4194153161 \% 6 = 5
$$

[[MongoDB|mongo]] and Cassandra use this

### Secondary Index Partitioning
Partitioning with secondary indexes adds considerable complexity, sithey don’t map neatly to partitions. There are two main approaches to partitioning a database with secondary indexes: document-based partitioning and term-based partitioning.

#### Document-based
Consider a website that lists cars. Each car is stored in its own document. We have fields like `year` and `color`. We want to give users the ability to query by color, so we add a secondary index to this field. Because of this index, whenever a red car is added to the database, the database partition automatically adds it to the list of document IDs for the index entry `color:red`.

Because the documents are partitioned based on their primary key, we now end up in a situation where we can have red cars existing across all partitions. However, because of the secondary index, we can still query by all of these cars in each partition. In effect, each partition only worries about its own secondary indexes. Naturally, partition1 doesn't care about the red cars existing in partition2. Because of this, a document-partitioned index is also known as a *local index* (as opposed to a *global index*).
- as mentioned, not all red cars exist on a single partition, so we need to query all partitions and combine the results (known as *scatter/gather*). Even if we query all the partitions in parallel, it is prone to tail latency amplification.
- used by [[mongo]], Riak, Cassandra, [[elastic-search]].

#### Term-based
Instead of each partition having its own secondary index (ie. local index), we can construct a *global secondary index* that covers all partitions.
- That global index must itself be partitioned, since it cannot live on a single node (which would defeat the purpose of partitioning)

In our car example above, imagine we had all cars with colors starting with *a* to *r* in partition 1, and *s* to *z* in partition 2.

This type of index is called *term-partitioned*, where the term would be the field:value that we are indexing (here, `color:red`)

The upside of term-based over document-based is that reads are made more efficient, since we don't have to *scatter/gather* over all partitions, and only need to query the partition(s) that have the particular term we are interested in.
- the downside is that writes are more complex and slower, since a single write may affect multiple partitions of the index (every term in the document might be on a different partition).

Updates to global secondary indexes are asynchronous.
- if you read the index shortly after a write, the change you just made may not yet be reflected in the index.

## Rebalancing
Rebalancing is the process of moving load (ie. data storage, read and write requests) from one node in the cluster to another. 
- The goal is to even out load across all nodes in the cluster.

To accomplish this smoothly, we should have many more partitions than we have nodes. If we have 10 nodes in the cluster, we may decide to have 1000 partitions, meaning each node holds 100 partitions. In the event that we add a node to the cluster, all we have to do is steal some partitions from each existing node and put them onto the new node until we reach a smooth distribution again. Removing a node is the same process in reverse.
- for this reason it's a good idea to set the number of partitions at the outset of setting up the database, and treat that number as unchanging. The idea is to choose a number that's high enough to accommodate growth (the number of partitions we have is the max amount of nodes we can have), but not so high that we are overwhelmed with the management overhead that comes with partitions.

Rebalancing is an expensive operation, because it requires rerouting requests and moving a large amount of data from one node to another. If it is not done carefully, this process can overload the network or the nodes and harm the performance of other requests while the rebalancing is in progress.
- automating rebalancing can be dangerous in combination with automatic failure detection. 
    - ex. say one node is overloaded and is temporarily slow to respond to requests. The other nodes conclude that the overloaded node is dead, and automatically rebalance the cluster to move load away from it. This puts additional load on the overloaded node, other nodes, and the network— making the situation worse and potentially causing a cascading failure.
        - this is why it's generally not a good idea to do a fully-automatic rebalancing.

Following the rebalancing process, data may no longer exist on the same partition as before, so the clients need to update the IP address that they will request data from. This general problem is called *service discovery*. To handle, we can...
- allow clients to contact any node (via a round-robin [[load balancer|deploy.distributed.load-balancer]]). If that node doesn't have the partition with the data requested, then it can forward the request on to the correct node, then receive the response and forward it back on to the client (like a proxy). 
- all requests will go first to a *routing tier* (basically a partition-aware load balancer), which determines which node the request should be sent to.
- require that clients know which node has the partition that holds the data it needs.

No matter which approach we take, the question then becomes: how do we propogate this new information about which nodes hold which partitions?
- For this, we can use a separate coordination system called ZooKeeper to keep track of this cluster metadata.
    - here, each node registers itself with ZooKeeper, and ZooKeeper maintains the authoritative mapping of partitions to nodes. Now, to know which node holds which partition, we only have to consult ZooKeeper.
- An alternative approach is to use a *gossip protocol* among the nodes to disseminate any changes in cluster state. Requests can be sent to any node,
and that node forwards them to the appropriate node.
- When using a routing tier or when sending requests to a random node, clients still need to find the IP addresses to connect to. These are not as fast-changing as the assignment of partitions to nodes, so it is often sufficient to use DNS for this purpose.

* * *

A partition is known as a *vnode* in Cassandra and Riak