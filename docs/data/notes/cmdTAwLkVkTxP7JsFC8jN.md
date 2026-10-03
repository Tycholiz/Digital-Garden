
Route53 is a [[dns]] provider.

It allows us to route end users to Internet applications by translating names like www.example.com to IP addresses.

effectively, Route53 connects user requests to infrastructure running in AWS, whether it be an [[aws.svc.EC2]] instance, [[aws.svc.S3]] bucket etc.
- it can also be used to route users to infrastructure outside of AWS.

Because Route 53 knows about AWS resources, you can do things like run health checks against load balancers, configure geo-routing based on specific AWS resources, etc.

A common way to do it would be to use Route53 for internal infrastructure, and something like Cloudflare for public facing resources.
