
Schema Registry provides a centralized repository for managing and validating schemas for topic messages
- It also provides a versioning mechanism so we can update the schema as needed.
- prior to interacting with the topic, the producer and consumer can each retrieve the latest version of the schema and validate their data against it.

Schema Registry also defines the format of the serialized data and is used to validate the data as it is being deserialized by another system or application
- That is, it is Schema Registry that knows if you serialized your Kafka messages with Avro, [[json-schema]] or Protobuf.