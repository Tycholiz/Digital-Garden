
## What i
CDK is an [[IaC|devops.IaC]] solution for provisioning AWS infrastructure that is written in common programming languages such as Javascript and Python

CDK is an alternative to [[terraform]]

Our CDK code will be transformed into [[CloudFormation|aws.svc.cloud-formation]] teplates, which are used to build our AWS infrastructure.

We use the CDK to manage infrastructure, while we use the [[AWS SDKs|aws.Sdk]] to interact with that infrastructure
- ex. you'd use the CDK to create a [[DynamoDB|aws.svc.dynamo]] table. You'd use the SDK to read and write data in that table.
