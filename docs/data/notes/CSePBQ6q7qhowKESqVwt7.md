
## Overview
Graphql is a protocol that gets implemented on a server, which gets communicated with from a client. The Graphql protocol defines a language for the client to use. The Graphql server is an expert at locating data. Through its resolvers, it knows exactly where every piece of data resides, and it knows how to get it through the most effective means. But in order to be able to know exactly which data you want, you have to speak to it in the graphql language.

GraphQL is useful when data is relational. Related data can be modeled as a graph and can thus power a GraphQL API. - This is precisely what the GraphQL Engine does.

Since Graphql is just a spec, we need to have an implementation to use it (similar to how SQL is a spec for Postgres, MySQL etc). The 2 components of a GraphQL implementation are: the GraphQL server and the GraphQL client.

Graphql has resolvers, each understanding where a certain resource of data is and how to get it. We might have data in a mongo database or a Postgres database, and as long as the resolver knows how to get it, Graphql doesn't really care. All it cares is that the data is retrievable.

To teach our Graphql server to be able to retrieve the data, we need to define a type system for it.

Graphql uses `POST` requests for queries and mutations under the hood, and websockets for subscriptions

<!-- TODO: broken image -->
<!-- ![2bc1e31d73c0da1e8598d766424dd751.png](:/a10e5211520a457fb318c12aec62d1ea) -->

### Phases
In GraphQL we have 3 phases:
1. *Parsing* - A [[document|graphql.documents]] is received by the Graphql engine and is parsed into an [[graphql.ast]], catching any syntax errors and stopping execution.
2. *Validation* - The Graphql server validates against the schema that we're only querying fields that are allowed, that fragments are applied on the right fields, etc.
	- note that this is handled for us, whereas if this was REST, it would be up to us.
3. *Execution* 

## Between the client and app server
A key thing to understand about GraphQL is that it’s actually agnostic to the way that data is transferred over the network
- ie. it can work on protocols other than HTTP, like websockets

* * *

## Selection Sets
A selection set in Graphql is similar to a result set in SQL, except the selection set is where we specify what we'd like to get back, while a result set in SQL is what we *actually* got back. 

As the name suggests, a selection set is a list of Selections (type Selection)
- a Selection has the following signature:
```gql
type Selection {
	Field
	FragmentSpread
	InlineFragment
}
```

A selection set might look like:
```gql
{
	id
	firstName
	lastName
}
```
Some fields may describe more complex data or relationships to other data. To allow us to explore this data, a field itself may contain a selection set.
- To remain unambiguous, the most nested fields must be scalars

### Distributed graphql
anal: We can have a Postgres database, Mongo database, Redis cache etc, and each is represented by a seed. The orange strand represents the means to access the data from that database. A distributed graphql solution essentially bundles the seeds together, and wraps the orange meat together, giving us a single means to access data from all the databases.
![](/assets/images/2021-12-02-10-49-11.png)

Distributed graphql can be achieved through schema stitching, [[apollo.federation]]

[more info](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-1-ae3557c187e2)

### UE Resources
[Graphql concepts visualized](https://www.apollographql.com/blog/the-concepts-of-graphql-bc68bd819be3/#.hfczgtdsj)
[security (preventing DDOS)](https://www.apollographql.com/blog/securing-your-graphql-api-from-malicious-queries-16130a324a6b/)
