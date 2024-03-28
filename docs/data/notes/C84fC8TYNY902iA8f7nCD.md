
Terragrunt is a tool on top of [[terraform]] for orchestrating multiple [[modules|terraform.module]] at a time
- Terragrunt helps organize [[state files|terraform.state]], help reduce code duplication, manage different AWS accounts gracefully.

- ex. We have an [[IAM Role|aws.terms.IAM]] for a Lambda function to run, but also have an S3 bucket for code to be deployed into. These 2 things aren't really related to each other, so we can put them into their own modules and then use Terragrunt to orchestrate all those modules and then deploy them all at once. 
    - Terragrunt sees all the modules it has to deploy (the terraform code) and figures out how to deploy it.

This is how we would implement the same `example_bucket` in both Terraform and Terragrunt.

Terraform:
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

Terragrunt:
```hcl
locals {
    env = "development"
    short_env = "dev"
    region = "us-east-1"
}

terraform {
    # This references a predefined S3 module. In this case, it is a repo called
    # S3 in the terraform-modules project group. The benefit is that there is a
    # lot of S3 code that would be the same across different S3 buckets. We can
    # define it once here, then reference it each time we create a new bucket.
    source = "git::https://gitlab.com/tycholiz/infra/terraform-modules/s3.git//?ref=0.3.0"
}

# These are inputs that are used by the source S3 bucket referenced above
# (terraform-modules/s3)
inputs = {
    bucket_name = "my-test-bucket-${locals.short_env}-${locals.region}-serverless"
    environment = locals.env
}
```

## UE Resources
- https://transcend.io/blog/why-we-use-terragrunt/
