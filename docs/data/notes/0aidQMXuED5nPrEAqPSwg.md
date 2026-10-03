
## Summary
A provider is a connection that allows Terraform to manage infrastructure using an interface (e.g. AWS API)

## How it works
The added abstraction means the provider is completely separate from the Terraform engine

We can have multiple instances of the same provider in our project. To more easily distinguish between the two, we can use an `alias` property.
- ex. if we want an aws provider in `us-east-1` and another in `us-west-2`

Because of the Terraform engine, anyone can write a provider to connect to anything that has a programmable way to talk to it.
- therefore, Providers are not part of the main Terraform source code. They are separate binaries that exist in their own repos.
    - Hashicorp hosts a registry that contains the most popular providers, like AWS and Azure

Providers in your project can be found in the `.terraform/` directory.
- each provider binary is in the format `terraform-provider-<NAME>_vX.Y.Z`
    - when you run `terraform init`, binaries of this format are automatically searched for so it can determine if it actually needs to download it or not. 

Terraform uses lockfiles so that the same versions will be used across different machines (just like npm/yarn lockfiles.)

All the author of a provider has to do for each resource they want Terraform to control is to define how to create, read and destroy it.
