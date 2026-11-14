
A topic is an ordered collection of events (ie. messages, records) stored in a durable way.
- A topic represents a single type of event
- Durable - written to disk, and replicated

A topic can be thought of as a feed name.

Messages in a topic must be first serialized so that it can be efficiently transmitted and stored in a Kafka topic.
- when you communicate with Kafka using a native client, that native client contains serializers that know how to serialize the data (in the case of producers) and deserialize the data (in the case of consumers)

A topic has a Log which is the topic’s storage on disk.
- a log is broken up into partitions and segments

Can be thought of as a *real-time stream*
- Topics can be thought of as a database table
  - ex. if we were doing this in Postgres, we could have tables `geofence_entry_events`, `low_tire_pressure_warning_events`

Kafka Services can be used as a sort of glue between microservices of an application. A microservice can consume a message from the Kafka Topic, and produce an output which gets registered to another Topic.
- Since it can act as glue between many services, we can produce an output from these Topics that can be consumed by a new service to perform real-time analysis of that data
  - This is contrast to the old-school method of running a batch-process overnight

Events in a topic are guaranteed to be in order (spec: by id)

### Partitioned nature of Topics
Topics are partitioned, meaning a topic is spread over a number of "buckets" located on different Kafka brokers.
- When a new event is published to a topic, it is actually appended to one of the topic's partitions
- Each partition in the Kafka cluster has a leader and a set of replicas among the brokers.

Kafka guarantees that any consumer of a given topic-partition will always read that partition's events in exactly the same order as they were written.

Events with the same event key (e.g., a customerId or vehicleId) are written to the same partition

Partition strategy: related events should be on the same partition