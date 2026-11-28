
### Resource
a resource is an entity that you can work with. Put another way, it is an instance of an AWS object.
- ex. [[EC2|aws.svc.EC2]] instance, a [[CloudFormation|aws.svc.cloud-formation]] stack, or an [[S3|aws.svc.S3]] bucket.

### Resource Group
Allow us to manage multiple resources together as a group, so we don't have to move from one AWS service to another for each task.
- If you manage large numbers of related resources, such as EC2 instances that make up an application layer, you likely need to perform bulk actions on these resources at one time
	- ex. applying updates/patches, upgrading applications, opening/closing ports to network traffic, collecting logs from machine etc.

All resources in a resource group are in the same AWS Region.

### ARN (Amazon Resource Name)
Uniquely identify an AWS resource. Therefore, it is identifiable across all of AWS.
- shows region and account number.

Format:
```
arn:partition:service:region:account-id:resource-type:resource-id
```

Example:
```
arn:aws:s3:::my_corporate_bucket/Development/*
arn:aws:ec2:us-east-1:4575734578134:instance/i-054dsfg34gdsfg38
```
