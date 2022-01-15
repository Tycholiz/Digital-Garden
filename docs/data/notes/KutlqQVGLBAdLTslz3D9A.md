
The AWS CDK is *only* for creating/deleting/managing infrastructure.
- It spits out [[CloudFormation|aws.svc.cloud-formation]] templates, which are what is used to build your AWS infra.

Essentially, if you are a developing an application, you will use the [[SDK|aws.SDK]] of your programming language. If you are creating infrastructure, you will use the CDK.

### vs SDK
You'd use the CDK to create a [[DynamoDB|aws.svc.dynamo]] table. You'd use the SDK to read and write data in that table.
