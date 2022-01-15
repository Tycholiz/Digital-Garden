
SDK is for interacting with AWS services for the most part.

The AWS SDK comes with a lot of low level code that is done already to interact with AWS services, so that you can include it in your app and do what you want without having to worry about what goes on underneath.
- So if you have an application written in JavaScript and it needs to process messages in an SQS queue, you would use the JavaScript SDK.
- It can also create/delete/manage infrastructure.

When used with AWS, Terraform is implemented using the AWS Go SDK.

### `lib` packages
When using AWS SDK, we may come across `lib` packages (e.g. `@aws-sdk/lib-dynamodb` instead of `@aws-sdk/client-dynamodb`). These packages are higher level.
- ex. in the `lib` version of DynamoDB package, Marshalling is handled for us.

### EventBridge Example
We could use the [@aws-sdk/client-eventbridge](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-eventbridge/index.html#aws-sdkclient-eventbridge) package to communicate with our [[EventBridge|aws.svc.event-bridge]].
- For instance, you can create rules that match selected events in the stream and route them to targets to take action.

Imagine you wanted to create a system where vehicle fleet managers can determine which types of event they are interested in (low tire pressure, enter/exit geofence etc). When the fleet manager indicates they want to be notified of low tire pressure, the act of them selecting that event would add a rule to the rules engine, effectively subscribing them to the `LowTirePressure` event. This is accomplished via code at runtime, rather than during compile/deploy time. Effectively, the rules engine can be modified on the fly, rather than having to do it ahead of time.
