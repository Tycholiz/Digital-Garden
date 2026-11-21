
AWS services are either Global, Regional or specific to the Availability Zone and cannot be accessed from outside.
- Most AWS managed services are regional based (except for [[IAM|aws.terms.IAM]], [[aws.svc.route53]], [[aws.svc.cloudfront]], WAF etc).

### Availability Zone
An Availability Zone (AZ) is a group of logical data centers, each one isolated from others.

Each Region has multiple AZs
- if a region might be `us-east-1`, the AZ might be `us-east-1a`
- note: other cloud providers might define a region as a single data centre.

Each AZ has independent and redundant power, cooling, and physical security and is connected via redundant, ultra-low-latency networks
- this means that a company running its resources in different AZs get the benefit of high availability and fault-tolerance, since AZs are isolated from each other. Therefore, it's a good strategy is to spread partitions across AZs
