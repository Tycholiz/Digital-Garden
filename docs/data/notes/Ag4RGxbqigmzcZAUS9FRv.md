
A function is a resource that you can invoke to run your code in Lambda. A function has code to process the events that you pass into the function or that other AWS services send to the function.

Functions can be invoked directly with...
- the Lambda console
- the Lambda API
- AWS SDK
- AWS CLI
- AWS toolkits

Functions can also be invoked *indirectly*...
- by configuring other AWS services to invoke it
	- ex. invoke your function when an object is created in [[S3|aws.svc.S3]]
- configuring Lambda to read from a stream or queue and invoke it.

### Sync vs Async Invocation
- With synchronous invocation, you wait for the function to process the event and return a response.
- With asynchronous invocation, Lambda queues the event for processing and returns a response immediately.
	- Therefore, in [[Node|js.node]] it returns a [[Promise|js.lang.promises]]

When you invoke a function synchronously, you receive errors in the response and can retry. When you invoke asynchronously, use an event source mapping, or configure another service to invoke your function, the retry requirements and the way that your function scales to handle large numbers of events can vary.

When you invoke a function asynchronously, you don't wait for a response from the function code. You hand off the event to Lambda and Lambda handles the rest. You can configure how Lambda handles errors, and can send invocation records to a downstream resource to chain together components of your application.

You can also configure Lambda to send an invocation record to another service. Lambda supports the following destinations for asynchronous invocation.
- an [[SQS|aws.svc.SQS]] queue
- an [[aws.svc.SNS]] topic
- a [[Lambda|aws.svc.lambda]] function
- Amazon EventBridge – An EventBridge event bus.

Several AWS services, such as [[S3|aws.svc.S3]] and [[SNS|aws.svc.SNS]] invoke functions asynchronously to process events.
