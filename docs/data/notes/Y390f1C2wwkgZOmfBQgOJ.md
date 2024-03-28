
The idea of Apollo Federation is that the organization should expose a single supergraph that provides a unified interface for querying all of our backing data sources. 
- However, we wouldn't want to do this with a single Graphql Server, so we'll string many subgraphs together into what's called a Gateway. This gateway acts as a proxy to the subgraphs.
![](/assets/images/2022-02-10-09-10-43.png)

The Federated approach differs from achieving the same result via *schema stitching*, since it uses a declarative programming model that enables each subgraph to implement only the part of your composed supergraph that it's responsible for.

Apollo Federation encourages [[Separation of Concerns|general.principles.SoC]], allowing different teams to work on different products/features within a single graph without interfering others.

Each subgraph should define the types and fields that it is capable of (and should be responsible for) populating from its back-end data store.
