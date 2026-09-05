
### Plan
`plan` will read the terraform HCL file and determine the desired state of the infrastructure, and compare that to the current infra.
- Once this "diff" is determined, `terraform plan` presents a description of the changes necessary to achieve that desired state (it does *not* actually make any changes).

If we want, we can save the plan as a runnable artifact that can be fed into the `terraform apply` command.

The purpose of the plan is to validate configuration changes and confirm that the resulting actions are as expected.

#### Flags
- `-out <filename>` - specify an output file for the diff

#### Markers in the plan diff
- `+` parts that will be added
- `-` parts that will be deleted
- `~` parts that will be updated in place (ie. it won't delete and recreate)
- `-/+` destroy-then-create
    - this erases the state and results in downtime. We can overcome this downtime by using a resource lifecycle

### Apply
`apply` will perform a plan just like `terraform plan` command, but then it will actually carry out the planned changes to each resource using the infrastructure provder's API (e.g. AWS API).
- therefore, it can be thought of as "committing" the plan.

### Destroy
`destroy` behaves exactly like deleting every resource from the configuration and then running an apply, except that it doesn't require editing the configuration
- Therefore, this is more convenient if you intend to provision similar resources at a later date.

* * *

### `import`
The `import` command is how we get Terraform to begin managing a resource that wasn't created in Terraform.
- What's happpening is that Terraform is fetching its data, then including it as part of its state.

```sh
terraform import <resource_type>.<resource_identifier> <value>
```