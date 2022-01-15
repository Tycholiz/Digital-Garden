
GraphQL is useful when data is relational. Related data can be modeled as a graph and can thus power a GraphQL API. - This is precisely what the GraphQL Engine does.

### Distributed graphql
anal: We can have a Postgres database, Mongo database, Redis cache etc, and each is represented by a seed. The orange strand represents the means to access the data from that database. A distributed graphql solution essentially bundles the seeds together, and wraps the orange meat together, giving us a single means to access data from all the databases.
![](/assets/images/2021-12-02-10-49-11.png)

Distributed graphql can be achieved through schema stitching, Apollo Federation

[more info](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-1-ae3557c187e2)

### UE Resources
[Graphql concepts visualized](https://www.apollographql.com/blog/the-concepts-of-graphql-bc68bd819be3/#.hfczgtdsj)
[security (preventing DDOS)](https://www.apollographql.com/blog/securing-your-graphql-api-from-malicious-queries-16130a324a6b/)
