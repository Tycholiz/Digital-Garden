
To be federated means to be unified in some way, while still maintaining the independence of each unit that lies underneath. The core outcome of a federated system is a group of loosely coupled components that share a common aim.
- ex. the idea of [[Apollo Federation|apollo.federation]] is that we have many Graphql endpoints (with their own upstream REST and database layers) tied together to provide the benefit of unity to the person who uses it.
- ex. [[Federated identity|security.federated-identity]] is the idea that we can link someone's identity across many platforms
- anal: consider that the United States is a federation. The key here is that each state maintains some degree of independence, yet is connected to the whole in some way.

### Federation in database
Federation (or functional partitioning) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag.

## Resources
- https://en.wikipedia.org/wiki/Federated_architecture