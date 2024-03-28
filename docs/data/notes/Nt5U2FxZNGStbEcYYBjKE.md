
## Summary
A resource represents a piece of real world infrastructure 

Examples:
- an S3 bucket
- an EKS (Elastic Kubernetes) cluster
- a Postgres [[role|pg.lang.roles]]

### Example: Declaring an S3 bucket
This is how we might declare an [[S3 bucket|aws.svc.S3]]. You can reference this in different components, and then everything will deploy together when running `terraform apply`.
```tf
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my-test-bucket"
    acl = "private"
    tags = {
        Name = "My bucket"
        Environment = "Dev"
    }
}
```