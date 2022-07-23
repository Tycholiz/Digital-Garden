
AWS SNS is a fully managed messaging service for decoupling event-driven microservices. 
- It enables us to send messages *reliably* between parts of the infrastructure.

SNS works via [[pub/sub|general.patterns.messaging.pubsub]] topics to allow services to consume relevant events.

SNS can be set up to send messages to end users via email, SMS, mobile push notifications etc.

SNS uses a robust retry mechanism in cases where downstream targets are unavailable.
- If it still fails, it will utilize a [[DLQ|general.patterns.messaging.DLQ]].

SNS does not retain messages so if there are no subscribers for a topic, the message is discarded.
- comparatively, an [[SQS|aws.svc.SQS]] message is stored on the queue for up to 14 days until it is successfully processed by a subscriber.

SNS uses topics to logically separate messages into channels, and your [[Lambda functions|aws.svc.lambda]] interact with these topics.

SNS topics may broadcast to multiple targets (fan-out).
- An SNS topic can have up to 12,500,000 subscribers, providing highly scalable fan-out capabilities.

SNS can be used to parallelize work across Lambda functions or send messages to multiple environments (such as test or development).

### vs [[EventBridge|aws.svc.event-bridge]]
Both can be used to decouple publishers and subscribers, filter messages or events, and provide fan-in or fan-out capabilities.

EventBridge offers two capabilities not in SNS:
- SaaS integration
- Schema registry
