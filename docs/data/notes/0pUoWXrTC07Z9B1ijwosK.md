
The Dead Letter Queue (DLQ) is a service that stores messages that meet one or more of the following criteria:
- message is sent to a [[queue|general.lang.data-structs.queue]] that doesn't exist
- queue/message length limit exceeded
- message is not processed successfully
- and others

Dead letter queue storing of these messages allows developers to look for common patterns and potential software problems

DLQs are incorporated out of the box with [[EventBridge|aws.svc.event-bridge]], [[SQS|aws.svc.SQS]], [[kafka]], RabbitMQ, and others
- [[EventBridge|aws.svc.event-bridge]] uses [[SQS|aws.svc.SQS]] as the DLQ
