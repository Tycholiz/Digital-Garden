
## Summary
A Terraform module is a set of Terraform configuration files in a single directory that can be considered its own standalone Terraform project.
- it can therefore...
    - contain its own resources, data sources, locals, etc.
    - take variables (ie. inputs on a per-module basis)

### What are they for?
Modules are useful as they allow us to define a reusable block of Terraform code of which we can have many instances in our main Terraform project.
- even a single directory with a single `.tf` file is considered a module.

A convention is to use a common prefix for resources from a single module

A Terraform module is similar to the concept of a package (e.g. npm package), providing many of the same benefits.
- As a result, any non-trivial real-world Terraform configuration would use modules.


Example:
```tf
module "work_queue" {
    source = "./directory-of-module"
    variable_name = "passed_in_value"
}
```

### Returning values from modules (`output`)
Most modules return values to make them easier to use.
- to reference those values, use `module.<module_identifier>.<output_name>`

It is possible to return a whole resource from a module. 
- This allows us to return all of the fields from the resource that we created

### Remote modules
Modules can be specified with a URL pointing to a remote git repo, designating them as *remote modulues*.
- these modules get cloned to `.terraform/modules/`, which is similar to `node_modules/` in [[js.node]]

Remote modules should be specified with a [[git tag|git.cli.tag]] to peg a specific version
- ex. `github.com/Tycholiz/my-terraform-module?ref=0.0.1`