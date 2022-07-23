
## What is it?
Terraform is a cloud-agnostic [[IaC|deploy.IaC]] solution.

Terraform is split into two parts:
- One part is the Terraform engine, which understands... 
    - how to read state from a provider
    - read HCL code
    - how to get from the state your infrastructure is currently into the state you want your infrastructure to be in.
- The other part is the provider, which talks to the infrastructure to find out the current state and make changes using the infrastructure’s API.

## Provisioning Workflow
There are 3 main CLI commands that involve creating, modifying and destroying infrastructure: `plan`, `apply` and `destroy`.

[[terraform.cli]]

## Terminology
### Provider
![[terraform.provider#summary,1:#*]]

### Resource
![[terraform.resource#summary,1:#*]]

### Module
![[terraform.module#summary,1:#*]]

### Data source
![[terraform.data-source#summary,1:#*]]

### Local
A `local` is Terraform's representation of a variable.
- note: not to be confused with Terraform variables.

### Variable
![[terraform.variables]]

### State
![[terraform.state]]

### Workspaces
Workspaces solve the problem "how do we create multiple environments using the same code?"

`terraform.workspace` is a special variable that resolves to the current workspace we are running in.

Unless we explicitly specify, we are running in the `default` workspace.

Local workspaces are stored in `terraform.tfstate.d/`
- each workspace has its own state

#### CLI
- List workspaces -  `terraform workspaces list`
- Create new workspace - `terraform workspace new development`
- Switch workspaces - `terraform workspace select development`

### Terraform Cloud
Terraform cloud provides us with a method to change our input variables at the top level, meaning each set of infra (for each environment) can have its own set of variables.

With it, we:
1. create a workspace 
2. point it at a source control repo containing your Terraform code
3. set the variables for that workspace

### Lifecycle
Each resource has a special attribute block called `lifecycle` that gives us extra control.

It allows us to:
- `create_before_destroy`, to ensure a new resource is created prior to deleting the old one
- `prevent_destroy`, to prevent Terraform from ever deleting the resource, so long as the property exists

```tf
lifecycle {
  prevent_destroy = true
}
```

### Provisioner
Provisioners allow us to run a script (remotely or locally) after a resource has been created.
- provisioners allow us to step in and solve problems ourselves when they are not solved out of the box by the provider we are using.
- because provisioners are imperative, they are seen as a last resort approach to solving our problem.

* * *

## Misc
### Multi-line string
Multi-line strings are declared between `<<ANYWORD` and `ANYWORD`:
```tf
resource "aws_iam_policy" "my_bucket_policy" {
    name = "my-bucket-policy"

    policy = <<POLICY
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "s3:ListBucket"
                ],
                "Effect": "Allow",
                "Resource": [
                    "${data.aws_s3_bucket.bucket.arn}"
                ]
            }
        ]
    }
    POLICY
}
```

String interpolation (`${interpolated_value}`) can be used inside a multi-line string.
- only needed when inside quotes (`""`)

### Outputting to console (stdout)
```tf
output "message" {
    value = aws_s3_bucket.my_bucket.id
}
```

or we can print all attributes exported by a resource:
```tf
output "all" {
    aws_s3_bucket.my_bucket
}
```

### Folder structure
All Terraform files should be in a single directory (the Terraform project) at the top level. Any files within subdirectories will be ignored. Conceptually, when we run Terraform commands, everything will be appended into a single file anyway.
- child directories are used to set up Modules

By convention,
- set up providers in `main.tf`.
- resources named after their type (e.g. `sqs.tf`, `api-gateway.tf`)
- variables in `variables.tf`

* * *

## Tools
- [Atlantis](https://www.runatlantis.io/) - Pull Request automation for Terraform
    - purpose is to have improved code review for infra changes.
- Terratest - a unit testing framework for Terraform

## Alternatives
- Chef/Puppet - these are configuration management tools. They are designed to configure and manage the already existing infrastructure, while Terraform is designed to set up the infrastructre itself.
    - In other words, Puppet and Chef would be used to configure servers, while Terraform would be used to create the server itself.
- Pulumi - This IaC tool uses a programming language (like Typescript) instead of a configuration language.