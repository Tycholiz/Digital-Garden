
State is the place where Terraform stores of all of the resources (and their metadata) it has created.
- run `terraform state list` to see all resources existing in state.

This state is used by Terraform to work out how changes need to be made.
- ex. imagine we did not use state. The first time we run `terraform apply`, our S3 bucket is created. Each subsequent time we ran that command, we would get `Resource already exists errors`, since we didn't have a state file to tell us what was there already. Due to the state file's absence, Terraform is assuming this is the first time, so it tries to create the resource. If we had a state file however, then Terraform would know that it has to check that state file to know if it is creating a new resource, updating an existing one, or simply doing nothing (because there would be no changes in state)

the output of the `terraform plan` command is a diff between the code on your computer and the infrastructure deployed in the real world, as discovered via IDs in the state file.

State is stored in `terraform.tfstate`
- Every time you run Terraform, it records information about what infrastructure it created in a Terraform state file.
- this file should not be committed to Git, since it may contain sensitive information. The recommended solution is to keep it in blob storage (such as [[S3|aws.svc.S3]])

note: The State File Is a Private API. The state file format is a private API that is meant only for internal use within Terraform. You should never edit the Terraform state files by hand or write code that reads them directly. If for some reason you need to manipulate the state file — which should be a relatively rare occurrence — use the terraform import or terraform state commands

If we want to move resource creation from one project to another, state needs to be manipulated directly 
- this can be handled by (example uses a AWS VPC resource)
    1. running `terraform state rm aws_vpc.my_vpc` command, which will remove the resource from state (so Terraform is no longer managing it), but will not delete the resource in the cloud.
    2. in the new project, copy+paste over the resource and run `terraform import aws_vpc.my_pc <VPC_ID>`
    3. run `terraform apply`

- some resources do not support `import`. In this case, use `terraform state mv`

### Remote state
If you’re using Terraform for a personal project, storing state in a single terraform.tfstate file that lives locally on your computer works just fine. But if you want to use Terraform as a team on a real product, you run into several problems:

- Shared storage for state files. To be able to use Terraform to update your infrastructure, each of your team members needs access to the same Terraform state files. That means you need to store those files in a shared location.
- Locking state files. As soon as data is shared, you run into a new problem: locking. Without locking, if two team members are running Terraform at the same time, you can run into race conditions as multiple Terraform processes make concurrent updates to the state files, leading to conflicts, data loss, and state file corruption.
- Isolating state files. When making changes to your infrastructure, it’s a best practice to isolate different environments. For example, when making a change in a testing or staging environment, you want to be sure that there is no way you can accidentally break production. But how can you isolate your changes if all of your infrastructure is defined in the same Terraform state file?

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
