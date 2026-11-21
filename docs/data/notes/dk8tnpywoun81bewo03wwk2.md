
The broker is the main vehicle for the movement of data in Kafka.
- it handles *all* requests from *all* clients (both producers and consumers)
- It also manages replication of data across a cluster, as well as within topics and partitions.

![](/assets/images/2023-06-26-21-52-48.png)

A Broker is a Kafka server that runs in a Kafka Cluster. Therefore, Kafka Brokers form a cluster. 
- A Kafka cluster is made up of multiple Kafka Brokers that exist on multiple servers.
- Kafka Brokers contain topic log partitions. 

Connecting to one broker bootstraps a client to the entire Kafka cluster. 
- For failover, you want to start with at least three to five brokers. A Kafka cluster can have, 10, 100, or 1,000 brokers in a cluster if needed.

The default port for the broker is `9092`

### Bootstrap server
We know that a kafka cluster can have 100s or 1000nds of brokers (kafka servers). But how do we tell clients (producers or consumers) to which to connect? Should we specify all 1000nds of kafka brokers in the configuration of clients? no, that would be troublesome and the list will be very lengthy. Instead what we can do is, take two to three brokers and consider them as bootstrap servers where a client initially connects. And then depending on alive or spacing, those brokers will point to a good kafka broker.
- these can be listed in `bootstrap.servers`

### Replication factor
Replication factor must be equal to or less than the number of brokers you have

https://www.educba.com/kafka-replication-factor/