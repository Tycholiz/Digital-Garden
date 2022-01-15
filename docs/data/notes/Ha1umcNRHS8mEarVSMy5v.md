
# SQS (Simple Queue Service)
Highly-durable queue in the cloud
- put messages on one end, and a consumer takes them out from the other side.

Messages are consumed *almost* in FIFO, but there is no strictness to adhere to this.
- Strictness *can* be guaranteed, but there is a performance cost.

Queues are an important mechanism for providing fault tolerance in distributed systems.
- The service durably persists messages until they are processed by a downstream consumer.

SQS scales elastically, and there is no limit to the number of messages per queue.

SQS requires zero capacity management.
- no limit on the rate of messages enqueued or consumed
- don’t have to worry about any throttling limits.
- number of messages stored in SQS (the backlog size) is also unlimited.

great default choice for dispatching asynchronous work.
