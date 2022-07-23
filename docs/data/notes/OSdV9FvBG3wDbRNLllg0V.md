
A Lambda is like an anonymous function (or a callback) that runs code in response to events.
- Think of them like event handlers, but for web services, not components within a webpage

Effectively, everything is abstracted away aside from a function interface.

## Concepts
### Function
![[aws.svc.lambda.fn]]

### Triggers
A trigger is a resource or configuration that invokes a Lambda function. Triggers include AWS services that you can configure to invoke a function and event source mappings.

A trigger can be configured to invoke your function in response to lifecycle events, external requests, or on a schedule.

Each trigger acts as a client invoking your function independently. Each event that Lambda passes to your function has data from only one client or trigger.

### Event
An event is a JSON-formatted document that contains data for a Lambda function to process. The runtime converts the event to an object and passes it to your function code.

![[aws.svc.lambda.event]]

## Lambda Runtime
Process:
1. Our Lambdas will be stored as zipped files in [[aws.svc.S3]].
2. The execution environment will be started (e.g. Node runtime, Python, C# etc.)
3. execute initialization code (the code outside the main `handler` function of the Lambda, which includes imports)
	- variables defined here will live in between invocations of our Lambda
4. execute the `handler` code

### Execution Environment
Lambda provides a secure and isolated runtime environment for your Lambda function. It manages the processes and resources that are required to run the function.

The execution environment provides lifecycle support for the function and for any extensions associated with your function.

[more on Lambda Execution Environment](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html)

### Deployment
You deploy your Lambda function code using a deployment package. Lambda supports two types of deployment packages:
- A .zip file archive that contains your function code and its dependencies.
- Your Lambda + its dependencies as a [[Docker image|docker.image]]. You can then load that container into something like AWS Elastic Container Registry (ECR)
	- Dockerizing Lambdas is a slower approach.

## Cold start
Only 1, 2 and 3 of the above process are relevant to cold starts.

Lambas will stay alive for ~5-30 min, but the time-frame is highly variable and unpredictable.

A cold start happens any time a new Lambda instance is created
- in other words, going from 0 Lambdas to 1 Lambda is a single cold start, just like going from 499 to 500 is a single cold start.

Cold starts are essentially unavoidable, and all we can do is reduce the frequency of their occurrence.

### Concurrency controls
There are 2 types of concurrency controls available: provisioned concurrency and reserved concurrency

#### Provisioned Concurrency
This is an AWS setting that allows us to keep a set number of Lambdas warm.
- there is a cost associated with keeping Lambdas warm, and if the load exceeds the number of Lambdas we've set to keep warm, we will still have cold starts.

#### Reserved Concurrency
This lets us guarantee a maximum number of concurrent instances for a function. When a function has reserved concurrency, no other function can use that concurrency.

* * *

### Lambda Extensions
We can include extensions to support our lambdas
![[aws.svc.lambda.extensions]]

### Concurrency
[[Concurrency|general.concurrency]] is the number of requests that your function is serving at any given time. When your function is invoked, Lambda provisions an instance of it to process the event. When the function code finishes running, it can handle another request. If the function is invoked again while a request is still being processed, another instance is provisioned, increasing the function's concurrency.

## Features
### Event Source Mapping
An event source mapping is a resource in Lambda that reads items from a stream or queue and sends the items to your function in batches.
- Each event that your function processes can contain hundreds or thousands of items.

Lambda reads events from [[Kinesis|aws.svc.kinesis]], [[Dynamoc|aws.svc.dynamo]], [[SQS|aws.svc.SQS]], among a few others.

You can use event source mappings to process items from a stream or queue in services that don't invoke Lambda functions directly

Event source mappings maintain a local queue of unprocessed items and handle retries if the function returns an error or is throttled.

### Destination
A destination is an AWS resource that receives invocation records for a function
- The invocation record contains details about the request and response in JSON format, including the function's response, and the reason that the record was sent.
	- this is something we'd want to send to the [[DLQ|general.patterns.messaging.DLQ]] in the event a message fails to be sent.

We can configure separate destinations depending on whether invocations pass or fail.

* * *

### How it works
Lambda provides a Programming Model, which is common to all runtimes (Node, C# etc). It defines the interface between your code and the Lambda system:
- You tell Lambda the entry point to your function by defining a handler in the function configuration. The runtime passes in objects to the handler that contain the invocation event and the context (such as the function name and request ID).

When the handler finishes processing the first event, the runtime sends it another.
- The function's class stays in memory, so clients and variables that are declared outside of the handler method in initialization code can be reused.
- Therefore, to save processing time on subsequent events, create reusable resources like AWS SDK clients during initialization. Once initialized, each instance of your function can process thousands of requests.


### When to use
Unless going with a serverless architecture, Lambda is most suitable for small snippets of code that rarely change.
- think of Lambda functions as part of the infrastructure rather than part of the application
	- The justification for adhering to this principle diminishes when we consider advancements like IaC (ex. [[Terraform|terraform]]).

Lambda can be used as a plugin system for other AWS services, for example:
- S3 doesn’t come with an API to resize an image after uploading it to a bucket, but with Lambda, you can add that capability to S3.

### Limitations
- Your functions will suffer a cold start when a function is invoked after a period of inactivity
- limit of 250 MB for your code bundle, including all your dependencies. This limit should not present an issue.

* * *

### Lambda Application
An AWS Lambda application is a combination of Lambda functions, event sources, and other resources that work together to perform tasks.
- With it, you can use [[CloudFormation|aws.svc.cloud-formation]] and other tools to collect your application's components into a single package that can be deployed and managed as one resource.

Applications make your Lambda projects portable

* * *

## Misc
Your function also has access to local storage in the /tmp directory. Instances of your function that are serving requests remain active for a few hours before being recycled.

The runtime captures logs from your function and sends it to Amazon CloudWatch Logs. We get:
- the return value of the function.
- entries detailing when functions were invoked, and when they failed.

Lambdas should be considered stateless, though we can use local storage and class-level objects to increase performance

By default, Lambda runs your functions in a secure VPC. Lambda owns this VPC, which isn't connected to your account's default VPC.
- When you connect a function to a VPC in your account, the function can't access the internet unless your VPC provides access.

You can configure a function to mount an Amazon Elastic File System (Amazon EFS) file system to a local directory. With Amazon EFS, your function code can access and modify shared resources safely and at high concurrency.

Lambda provides runtimes for .NET (PowerShell, C\#), Go, Java, Node.js, Python, and Ruby.

It may take 5 to 10 minutes for logs to show up in [[Cloudwatch|aws.svc.cloudwatch]] after a Lambda function invocation.

Lambdas are billed by GB/second, which means if we allocate more memory to our Lambdas, the cost/second will be higher.

When using [[CommonJS|js.lang.imports#commonjs-imports,1:#*]], barrel files (`index.js` file that `exports` all of the modules within the same directory) are potentially problematic with projects that implement Lambdas, since every module exported from the `index.js` file will be imported when we try to import a module.

## Resources
- [always allocate at least 1024mb](https://blog.neverendingqs.com/2021/04/16/aws-lambda-always-allocate-1024-mb.html)