
# CloudFormation
When working in AWS, you almost always want to use some CloudFormation (or a similar tool).
It lets you create and update the things you have in AWS without having to click around on the console or write fragile scripts.
- for instance, gives us the ability to tear down everything cleanly and recreate your AWS set up in one click
- spec: the alternative to using CloudFormation is doing click-ops

rule of thumb is to let CloudFormation deal with all the AWS things that are either static or change very rarely, like load balancers, deployment pipelines, VPC configs, security groups

### Overview
- define your AWS resources as a YAML script
- point CloudFormation to your AWS account, and it creates all the resources you defined idempotently.
- Updates can be rolled back.

While CloudFormation is specific to AWS, we can use [[Terraform|terraform]], which is platform agnostic.

stack refers to all the lamdbas that are found in the projet
- for instance, if we are using serverless framework, the stack refers to all of the stuff in serverless.yml
if we are looking for stacks, dt-fleet-event-bus stack
