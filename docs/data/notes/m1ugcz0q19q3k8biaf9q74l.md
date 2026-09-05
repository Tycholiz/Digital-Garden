
If you see the documentation after running a command, this means your command is wrong. Scroll up to see details about the error message

`--zookeeper` option is outdated and shouldn't be used
- Since Kafka v0.10, the consumer is leveraging a Kafka connection string, not Zookeeper. This is due to how consumer offsets are stored.

## `kafka-topics`
### Create topic
```sh
kafka-topics --create \
  --bootstrap-server localhost:9092 \
  --replication-factor 1 \
  --partitions 3 \
  --topic my_topic
```

### List topics
```sh
kafka-topics --list --bootstrap-server localhost:9092
```

### Describe a topic
```sh
kafka-topics --bootstrap-server localhost:9092 --describe --topic my_topic
```

### Increase # of partitions of a topic
```sh
kafka-topics --bootstrap-server localhost:9092 --alter --topic my_topic --partitions 5
```

### Delete a topic
```sh
# You can specify a comma delimited list of topics to delete more than one topic at a time
kafka-topics --bootstrap-server localhost:9092 --delete --topic my_topic
```

## `kafka-console-producer`
### Produce message
By default messages sent to a Kafka topic will result in messages with `null` keys. We must use the properties parse.key and key.separator to send the key alongside messages.

```sh
kafka-console-producer --bootstrap-server localhost:9092 --topic my_topic
# any line of text you write after the '>' will be sent to the Kafka topic

# or, pass messages (newline separated) from a file
kafka-console-producer --bootstrap-server localhost:9092 --topic my_topic < topic-input.txt
```

## `kafka-console-consumer`
### Consume only future messages
```sh
kafka-console-consumer --bootstrap-server localhost:9092 --topic my_topic
```

### Consume all historical messages
```sh
kafka-console-consumer --bootstrap-server localhost:9092 --topic my_topic --from-beginning
```