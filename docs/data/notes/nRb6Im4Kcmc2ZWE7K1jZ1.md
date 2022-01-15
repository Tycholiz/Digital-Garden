
The idea with Serverless Framework is "let's think of our application in terms of events and functions. We recognize when events happen, and we know which functions to run when they do."

Serverless is an IaC tool

To accomplish this philosophy, the framework defined a `serverless.yml`
- handles the logic of which function ([[Lambda|aws.svc.lambda]]) to call, and when to call it.
	- ex. we made decide to call that function when a particular event enters an [[event bus|aws.svc.event-bridge]]. In this case, all we need to do in the `serverless.yml` is to specify the aws resourceId (ARN) of the event bus, and it will be subscribed to.

Serverless framework is cloud-provider agnostic; all examples just assume AWS

What the Serverless framework does is that it automates the process of creating all the necessary services on the cloud provider. Once you run sls deploy the config file, which is called `serverless.yml` is transformed into a [[CloudFormation|aws.svc.cloud-formation]] template.
- once the `serverless.yml` has been transformed into a CloudFormation template it gets sent to AWS to create the services you specified; all API Gateway routes, all Lambdas etc.
- Apart from that, the Serverless framework also packages up all the code you have written and sends it to an [[S3|aws.svc.S3]] bucket.
	- This bucket will have all the versions of the code you have ever deployed. Effectively, the Serverless framework grabs the latest version from the bucket and sends it to the Lambda it created.

### `serverless.yml`
The yml file is a configuration file that declaratively describes all the necessary services that should be deployed to the cloud provider.

#### `functions`
Each function in this object basically says: "when an event comes through with these header properties, run this function."

* * *

The serverless framework is just one of many ways to use serverless architectures. Alternatives include:
- Managing lambdas yourself in AWS's console (or equivalent for other platforms like Azure, etc)
- Building & deploying your application with chalice AWS Lambda & Python ONLY
- deploying your application with apex (AWS ONLY)
- deploy a traditional Python WSGI app to AWS using Zappa
- lots of other options.

### Stage
using this to map to environments, resulting in a different environment variables that we have our build in.

### Alternatives
- AWS SAM
- AWS [[CDK|aws.CDK]]
	- essentially the same thing, but using a code language instead of Yaml

## Example
serverless.yml
```yml
service: The name of my app

provider:
	name: aws
	runtime: nodejs14.x

functions:
	MyLambda:
		handler: src/my-lambda.handler
		events:
			- http:
				path: hello/world
				method: get
```

src/my-lambda.ts
```ts
import type { APIGatewayProxyEvent } from 'aws-lambda'
export async function handler(event: APIGatewayProxyEvent) {
	return {
		statusCode: 200,
		body: 'Hello world'
	}
}
```

Then run `serverless deploy`
