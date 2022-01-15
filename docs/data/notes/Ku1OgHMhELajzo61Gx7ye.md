
Cloud-agnostic [[IaC|deploy.IaC]] solution.

Terraform uses HCL which is like JSON. This file sets up the infra. we can define services to deploy infra services in a declarative manner (e.g. EC2 instances)
- ex. this is the EC2 instance I want done. you define this in the config file, and then it goes and deploys it.
when we deploy the code, we deploy it into that infra environment.

This is how we might declare an S3 bucket. You can reference this in different components, and then everything will deploy together when running `terraform apply`.
```tf
resource "aws_s3_bucket" "example_bucket" {
    bucket = "my-test-bucket"
    acl = "private"
    tags = {
        Name = "My bucket"
        Environment = "Dev"
    }
}
```

The EKS cluster (K8S) is defined in Terraform.


# Terragrunt
terragrunt is an open source tool on top of terraform used to operate multiple modules at a time

Terragrunt is for orchestrating multiple Terraform modules at once
- ex. We have an [[IAM Role|aws.terms.IAM]] for a Lambda function to run, but also have an S3 bucket for code to be deployed into. These 2 things aren't really related to each other, so we can put them into their own modules and then use Terragrunt to orchestrate all those modules and then deploy them all at once. 
    - Terragrunt sees all the modules it has to deploy (the terraform code) and figures out how to deploy it.

This is how we would implement the same `example_bucket` above in Terragrunt
```hcl
locals {
    env = "development"
    short_env = "dev"
    region = "us-east-1"
}

terraform {
    # this references a predefined S3 module. In this case, it is a repo called s3 in the terraform-modules project group. The benefit is that there is a lot of s3 code that would be the same across different s3 buckets. We can define it once here, then reference it each time we create a new bucket.
    source = "git::https://gitlab.com/tycholiz/infra/terraform-modules/s3.git//?ref=0.3.0"
}

# These are inputs that are used by the source s3 bucket referenced above (terraform-modules/s3)
inputs = {
    bucket_name = "my-test-bucket-${locals.short_env}-${locals.region}-serverless"
    environment = locals.env
}
```
