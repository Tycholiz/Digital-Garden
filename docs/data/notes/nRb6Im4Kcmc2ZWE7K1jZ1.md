
The idea with Serverless Framework is "let's think of our application in terms of events and functions. We recognize when events happen, and we know which functions to run when they do."

Serverless is an [[IaC|devops.IaC]] tool
- the framework is cloud-provider agnostic; all examples just assume AWS

To accomplish this philosophy, the framework defines a `serverless.yml`
- handles the logic of which [[lambda|aws.svc.lambda]] to call, and when to call it.
	- ex. we may decide to call that function when a particular event enters an [[event bus|aws.svc.event-bridge]]. In this case, all we need to do in the `serverless.yml` is to specify the aws resourceId (ARN) of the event bus, and it will be subscribed to.

What the framework does is that it automates the process of creating all the necessary services on the cloud provider. Once you run `sls deploy` the config file (`serverless.yml`) is transformed into a [[CloudFormation|aws.svc.cloud-formation]] template.
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

### `stage`
using this to map to environments, resulting in a different environment variables that we have our build in.

### `package`
"Packaging" refers to the process of assembling the code and dependencies required for your serverless functions into a deployable artifact, which can then be easily uploaded to AWS.

By default, when you deploy a Serverless service, all of your functions share a single deployment package. 
- This means that all functions in your service will be packaged together into a single ZIP file, and this ZIP file is then uploaded to AWS Lambda.
- We can set `individually: true` to override this behaviour
	- with this option, each function is packaged separately into its own ZIP file and uploaded to AWS Lambda.

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

* * *

Serverless framework supports [[Cloudflare workers|cloudflare.workers]]
- build serverless applications that are then deployed as Cloudflare Workers.
- For developers whose applications run code in multiple places, using the Serverless Framework may be more efficient than writing their Workers within the Cloudflare Workers UI.

# Alternatives
- [[AWS SAM|aws.svc.sam]] & [[AWS CDK|aws.CDK]]
	- essentially the same thing, but using a code language instead of Yaml
