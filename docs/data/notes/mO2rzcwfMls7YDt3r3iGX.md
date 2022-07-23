
## Summary
A data source is used to fetch data from a resource that is not managed by the current Terraform project.
- think of it as a read-only resource that already exists

### How is it used?
The following data source is used to look up an [[aws.svc.S3]] bucket.
```hcl
data "aws_s3_bucket" "bucket_identifier" {
    bucket = "my_s3_bucket"
}
```

We could use a *data source expression* to access values of the data source, such as the [[arn|aws.terms#arn-amazon-resource-name]] of the above bucket, like `data.aws_s3_bucket.bucket_identifier.arn`
- ex. we could then use this in our [[IAM|aws.terms.IAM]] policy

The bigger the project, the more valuable data sources are, since we can split up our terraform project into multiple projects and reference them via data sources.
- the benefit is two-fold, since not only do we *not* have to hard-code things like ARNs, but if the resource that we are referencing no longer exists, then the Terraform will properly fail on us.
