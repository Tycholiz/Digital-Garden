
Provisioners are an escape-hatch method of performing actions that fall outside the regular lifecycle of Terraform.
- Terraform cannot model the actions of provisioners as part of a plan because they can in principle take any action.
- successful use of provisioners requires coordinating many more details than Terraform usage usually requires: direct network access to your servers, issuing Terraform credentials to log in, making sure that all of the necessary external software is installed, etc.

Provisioners are used to execute scripts on a local or remote machine as part of resource creation or destruction. Provisioners can be used to bootstrap a resource, cleanup before destroy, run configuration management, etc.

## Use-cases
Even though provisioners can be used to achieve the following use-cases, they still are not the preferred way.

- Passing data into virtual machines and other compute resources

## Provisioner types
### `file`
https://developer.hashicorp.com/terraform/language/resources/provisioners/file
### `local-exec`
https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec
### `remote-exec`
https://developer.hashicorp.com/terraform/language/resources/provisioners/remote-exec