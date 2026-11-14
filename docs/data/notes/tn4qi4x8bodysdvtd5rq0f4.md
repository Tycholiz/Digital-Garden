
## Overview
AWS SAM is a toolkit around improving the developer experience of building and running serverless apps.

Consists of 2 primary parts:
1. SAM template specification
2. SAM CLI

### SAM Template Specification
An open-source framework for defining and managing our serverless application infrastructure code.

SAM template specification is built on [[CloudFormation|aws.svc.cloud-formation]]
- therefore we can use AWS CloudFormation syntax directly within our SAM template
- but because it is an extension of CloudFormation, SAM offers its own unique syntax to be combined with the syntax of CloudFormation.

To provision the infrastructure specified in the template, it is deployed to CloudFormation where the SAM template syntax will be transformed into CloudFormation syntax.
- 23 lines of SAM template code will produce 200+ lines of CloudFormation code.

#### Example: Simple READ app
Imagine we want to make a simple application consisting of a lambda (exposed through API Gateway) which gets some records from DynamoDB.

Our application's SAM template can be made to define:
- the lambda
- the API Gateway
- the DynamoDB
- the [[IAM|aws.terms.IAM]] permissions necessary for these services to interact with one another

### SAM CLI
SAM CLI is a command line tool to be used with SAM templates to build and run serverless applications.
- Quickly initialize a new application project.
    - `sam init`
- Build your application for deployment, generating the `.aws-sam` directory (which includes all of our code and dependencies)
    - `sam build`
- Perform local debugging and testing, including allowing us to simulate events, test APIs, invoke functions, etc.
    - `sam local invoke`
- Deploy your application.
    - `sam deploy --guided`
- Configure CI/CD deployment pipelines.
- Monitor and troubleshoot your application in the cloud.
    - `sam list` to view deployed resources
    - `sam logs`
- Sync local changes to the cloud as you develop.
    - `sam sync --watch` to automatically detect changes and deploy updates to the cloud

SAM CLI can also work with third-party products such as [[Terraform|terraform]]
