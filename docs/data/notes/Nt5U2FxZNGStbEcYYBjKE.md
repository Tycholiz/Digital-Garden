
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

## Null Resource
A `null_resource` is similar to a regular resource, in that it adheres to the resource lifecycle model. 
- However, it merely serves as a placeholder for executing arbitrary actions within Terraform configurations, but without actually provisioning any physical resources.
- It does not perform any further actions beyond initialization
- The default behavior of a `null_resource` is that it will execute *only once* during the **first** run of the terraform apply command, but this can be overridden with the `triggers` field.

Below is the sample code:

```tf
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo This command will execute only once during apply"
  }
}
```

If we run `terraform apply` on this module, we will see the `echo` command in the console. However, if we run it again, that same `echo` command will not be run.

If we want the block to run every time, we can add:
```ts
triggers = {
    always_run = timestamp()
}
```