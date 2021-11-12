
A Resource group is a logical container for related resources. We would group together resources that we want to manage together as a group.
- all resources in a RG should be a part of the same lifecycle, meaning we deploy, update, and delete them together.
- resource groups can be stored in a different location than the resource that is comprises. This is possible because the location of the RG is merely the location of the RGs metadata about its resources.
- created with command `az group create`
- ex. `AzureFunctionsQuickstart-rg`
