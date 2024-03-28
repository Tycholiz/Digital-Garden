
To be federated means to be unified in some way, while still maintaining the independence of each unit that lies underneath. The core outcome of a federated system is a group of loosely coupled components that share a common aim.
- ex. the idea of [[Apollo Federation|apollo.federation]] is that we have many Graphql endpoints (with their own upstream REST and database layers) tied together to provide the benefit of unity to the person who uses it.
- ex. [[Federated identity|security.federated-identity]] is the idea that we can link someone's identity across many platforms
- anal: consider that the United States is a federation. The key here is that each state maintains some degree of independence, yet is connected to the whole in some way.

"to federate" means to establish a trusted connection or authentication between two different systems or entities. It typically involves allowing one system to access resources or perform actions on another system using the credentials or authorization provided by the first system.
- ex. "Gitlab has a JWT_TOKEN environment variable that's passed from each job which can be used to federate into your AWS account securely"
    - here, the JWT_TOKEN from GitLab is being used as a secure method to authenticate and access resources in the AWS account without needing to expose or manage AWS-specific credentials separately.

### Federation in database
Federation (or functional partitioning) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag.

## Resources
- https://en.wikipedia.org/wiki/Federated_architecture