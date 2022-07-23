
A message broker (also known as a message queue) is essentially a kind of database that is optimized for handling message streams.

The message broker runs as a server, with both producer and consumers connecting to it as clients.
- Producers write messages to the broker, and consumers receive them by reading them from the broker.
- By centralizing the data in the broker, these systems can more easily tolerate clients that come and go (connect, disconnect, and crash), and the question of durability is moved to the broker instead.

Using a broker is an alternative to using direct network communication between producers and consumers without going via intermediary nodes.
- forgoing a broker is done in situations where latency is very important, such as stock market feeds.

Since networks are unreliable, forgoing a message broker requires the application code to be aware of the possibility of message loss.
- even if the protocols detect and retransmit packets that are lost in the network, they generally assume that producers and consumers are constantly online.
- If a consumer is offline, it may miss messages that were sent while it is unreachable. Some protocols allow the producer to retry failed message deliveries, but this approach may break down if the producer crashes, losing the buffer of messages that it was supposed to retry.

most message brokers automatically delete a message when it has been successfully delivered to its consumers

Consumers may crash at any time, so it could happen that a broker delivers a message to a consumer but the consumer never processes it, or only partially processes it before crashing. In order to ensure that the message is not lost, message brokers use *acknowledgments*: a client must explicitly tell the broker when it has finished processing a message so that the broker can remove it from the queue.
- If the connection to a client is closed or times out without the broker receiving an acknowledgment, it assumes that the message was not processed, and therefore it delivers the message again to another consumer.

message brokers often support some way of subscribing to a subset of topics matching some pattern.

When multiple consumers read messages in the same topic, two main patterns of messaging are used:
1. Load balancing - In an effort to parallelize the processing, each message is delivered to one of the consumers so the consumers can share the work of processing the messages in the topic.
2. Fan-out - Each message is delivered to all of the consumers.

The two patterns can be combined: for example, two separate groups of consumers may each subscribe to a topic, such that each group collectively receives all messages, but within each group only one of the nodes receives each message.