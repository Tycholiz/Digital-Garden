
A consumer is a client application that subscribes to (reads and processes) events
- it works by issuing "fetch" requests to the brokers leading the partitions it wants to consume.

### Offset
The offset is like a bookmark within a partition of a topic, representing the consumer's current position in a log of messages.
- Each message in a Kafka topic is assigned a unique offset as it is written to the partition.
- Each request to the brokers specifies the offset, and consumers are able to rewind it to re-consume data if desired.
- Offsets are persistent and maintained by the Kafka broker.
- Offsets are used to provide the "at-least-once" delivery guarantee in Kafka.
- Typically, a consumer will persist the offset so that in case of failure, they can resume processing from where they left off.

### Consumer Groups
In a large-scale system, it is unlikely that we would have a single instance of a Kafka Consumer app. If we were using Kubernetes, we would have multiple pods of this application. The question then becomes: "how do we ensure that there is no overlap between each pod and the [[kafka.topic]] messages it consumes?"

The purpose of consumer groups in Kafka is to enable (horizontally) scalable and parallel consumption of messages from topics.
- Also, consumer groups provides scalability, fault tolerance, and load balancing for your Kafka consumers within the Kafka Consumer app. If a pod fails or a new pod is added, Kafka will automatically handle the partition reassignment to ensure efficient consumption and fault tolerance within the consumer group.

consumer group coordination is done based on the `group.id` configuration parameter.
