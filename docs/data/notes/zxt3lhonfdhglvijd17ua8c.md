
Zookeeper is a fault-tolerant distributed coordination service

Zookeeper and Kafka work in conjunction to form a Kafka Cluster.

Note: Zookeeper is set to be deprecated and replaced by [Kafka Raft](https://www.openlogic.com/blog/kafka-raft-mode)

## Why use it?
In general, ZooKeeper provides an in-sync view of the Kafka cluster.  

Zookeeper solves the same problem as [[k8s.node.master.components.etcd]]: distributed system coordination and metadata storage

### Metadata management
ZooKeeper stores metadata about the Kafka cluster, including information about brokers, topics, partitions, and consumer group coordination. It maintains a hierarchical namespace in a distributed and replicated manner, allowing Kafka to store and retrieve metadata efficiently.

### Leader election
ZooKeeper helps with leader election of brokers in Kafka. Each partition in Kafka is handled by a leader broker, and ZooKeeper is responsible for selecting and maintaining the leader for each partition. In case a leader fails, ZooKeeper facilitates the process of electing a new leader for the partition.

### Cluster Membership
ZooKeeper keeps track of the live brokers in the Kafka cluster. It allows brokers to register themselves and provides a mechanism for detecting when brokers join or leave the cluster. This information is crucial for maintaining the overall health and availability of the Kafka cluster.

### Topic Configuration and Partition Assignment
ZooKeeper helps with topic configuration and partition assignment. It stores the topic configurations, such as the number of partitions and replication factor, and ensures that the partitions are evenly distributed across the brokers in the cluster.

### Consumer Group Coordination
ZooKeeper is used to manage consumer group coordination in Kafka. It keeps track of the consumer groups, their assigned partitions, and the offsets consumed by each consumer group. This enables Kafka to ensure that each consumer group is processing messages consistently and allows for fault tolerance and load balancing.

### Watchers and Notifications
ZooKeeper provides a mechanism called watchers, which allows clients to receive notifications when certain events occur in the ZooKeeper cluster. Kafka leverages these watchers to be notified of changes to the cluster, such as broker failures or topic configuration updates.

## How does it work?
### Znode
- short for "Zookeeper Node"

The znode is the fundamental data entity that represents a point in the ZooKeeper's distributed hierarchical namespace tree.
- Znodes can have data associated with them
- It can be created, modified, deleted, and queried by clients interacting with the ZooKeeper service.

Znodes form the foundation for implementing distributed locking, leader election, configuration management, and other distributed coordination patterns.

It is similar to a file or directory in a traditional file system, but it is stored in memory and allows for coordination and synchronization among distributed systems.
- similar to the path of a file or directory, each znode has a unique path within the namespace

* * *

## Other
Besides Kafka, Zookeeper is also utilized by services such as [[apache.hadoop]], HBase, SOLR, Spark, and NiFi among others.