
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

### SQS vs SNS
SNS is "heavier":
- SQS lets any authorized connection read from it, SNS requires a specific subscription. 
- SQS can be read from a local dev end, SNS will need a local tunnel like ngrok to receive. 
- By default stuff just waits on SQS, the default SNS behaviour is to immediately send the notification.