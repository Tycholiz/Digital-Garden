
Azure gives you many different ways to interact with its resources, such as Azure Portal, Azure SDKs, az (command-line tool) etc.

## Deployment
- To deploy Functions to the cloud, we need to create 3 different resources: Resource group, storage account, and function app

### Storage Account
- maintains state and other information about your projects
- created with command `az storage account create`

### Function app
- provides the environment for executing your function code
- A function app maps to your local function project and lets you group functions as a logical unit for easier management, deployment, and sharing of resources
- created with command `az functionapp create`
	- When running this command, we also determine the `<APP_NAME>`. This will serve as the default DNS domain for the function app (ie. we invoke the function with an HTTP request to `<APP_NAME>.azurewebsites.net`)

* * *

## Artifacts
- Artifacts allows us to create/share/manage dependencies (ex. npm packages) in our project (client-read, client-publisher)
	- usually we explicitly define each feed that we pull our package from. Artifacts allows us to use a single point of entry for multiple existing feeds.
- Github Package Manager is analogous to Artifacts

Artifact Feed
- "feed" is an Azure-specific term
- a feed is a construct that allows us to store, manage and group packages (like npm), and control who we share it with.
- a feed is like an npm registry, similar to an endpoint that specifies where these packages can be found.
- a feed gives us access to a collection of packages.
- a feed is made up of artifacts

Build Artifacts
- build artifact is the output of running the azure-pipeline.yml CI file.
- Build Artifacts are different than Artifacts
- ex. build artifact is the result of taking some input data, processing it in some way, then stamping it with a commit SHA as well as a build #, letting us track it.

Azure has 2 types of pipelines: build pipeline and release pipeline.
- the build pipeline is CI (build, test and create artifacts), while release pipeline is CD
* * *

### Resource Manager
When a user sends a request from any of the Azure tools, APIs, or SDKs, Resource Manager receives the request. Then it:
1. authenticates and authorizes the request
2. sends the request to the Azure service
- since it is a central API that all requests pass through, we can see logs and everything related to these services and the requests they receive.
- the Resource Manager allows us to declaratively manage resources (as opposed to having to use scripts)

#### Scope
Azure provides four levels of scope: management groups, subscriptions, resource groups, and resources.
![](/assets/images/2021-03-08-21-28-23.png)
- management settings can be applied to any of these levels of scope, and the level we choose will determine how widely the setting is applied (lower levels inherit from higher).

## Terminology
**Resource**
- Any manageable item that is available through Azure.
- ex. VMs, storage accounts, web apps, databases, blob storage, virtual networks, resource groups, management groups etc.

## Questions
- What would the connection string be a connection to?
	- would it be the host of the function?
