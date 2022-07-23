
State is the place where Terraform stores of all of the resources (and their metadata) it has created.
- run `terraform state list` to see all resources existing in state.

This state is used by Terraform to work out how changes need to be made.

State is stored in `terraform.tfstate`

If we want to move resource creation from one project to another, state needs to be manipulated directly 
- this can be handled by (example uses a AWS VPC resource)
    1. running `terraform state rm aws_vpc.my_vpc` command, which will remove the resource from state (so Terraform is no longer managing it), but will not delete the resource in the cloud.
    2. in the new project, copy+paste over the resource and run `terraform import aws_vpc.my_pc <VPC_ID>`
    3. run `terraform apply`

- some resources do not support `import`. In this case, use `terraform state mv`

### Remote state
Multiple people working on the same Terraform project can introduce a lot of complexity, since a local state file is used to store a record of what has been created. If we run terraform commands on a second machine, it will try to create double the resources.
- to get around this issue, we can store state in a remote location (e.g. in an S3 bucket)

We specify the remote state location using the `backend` keyword. Here we are using an S3 bucket:
```tf
# state.tf
backend "s3" {
    bucket = "<bucket-name>"
    key = "my-project.state"
    region = "us-west-1"
}
```

The remote state backend needs to support "locking", which prevents changes to the state while Terraform commands are running.

A good idea is to use S3 bucket versioning so we can time travel through different Terraform states.
