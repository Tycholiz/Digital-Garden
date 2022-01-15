
# Kinesis
A highly-durable linked list in the cloud

Use-cases are similar to SQS— you would typically use either Kinesis or SQS when you want to enqueue records for asynchronous processing.

Difference with SQS:
- SQS can only have one consumer, while Kinesis can have many.
- Once an SQS message gets consumed, it gets deleted from the queue. But Kinesis records get added to a list in a stable order, and any number of consumers can read a copy of the stream by keeping a cursor over this never-ending list.
- Multiple consumers don’t affect each other, and if one falls behind, it doesn’t slow down the other consumers.
- Whenever consumers read data out of Kinesis, they will always get their records in the same order.
- Often cheaper than SQS
- Kinesis can carry a significant operational burden with the need to provision capacity (shards).
