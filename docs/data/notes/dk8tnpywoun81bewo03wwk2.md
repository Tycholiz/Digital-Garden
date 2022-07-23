
A Broker is a Kafka server that runs in a Kafka Cluster. Therefore, Kafka Brokers form a cluster. 
- A Kafka cluster is made up of multiple Kafka Brokers that exist on multiple servers.
- Each Kafka Broker has a unique ID. 
- Kafka Brokers contain topic log partitions. 

Connecting to one broker bootstraps a client to the entire Kafka cluster. 
- For failover, you want to start with at least three to five brokers. A Kafka cluster can have, 10, 100, or 1,000 brokers in a cluster if needed.

The default port for the broker is `9092`