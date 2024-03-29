
# Overview
Graphql is a protocol that gets implemented on a server, which gets communicated with from a client. The Graphql protocol defines a language for the client to use. The Graphql server is an expert at locating data. Through its resolvers, it knows exactly where every piece of data resides, and it knows how to get it through the most effective means. But in order to be able to know exactly which data you want, you have to speak to it in the graphql language.

Since Graphql is just a spec, we need to have an implementation to use it (similar to how SQL is a spec for Postgres, MySQL etc). The 2 components of a GraphQL implementation are: the GraphQL server and the GraphQL client.

Graphql has resolvers, each understanding where a certain resource of data is and how to get it. We might have data in a mongo database or a Postgres database, and as long as the resolver knows how to get it, Graphql doesn't really care. All it cares is that the data is retrievable.

To teach our Graphql server to be able to retrieve the data, we need to define a type system for it.

Graphql uses `POST` requests for queries and mutations under the hood, and websockets for subscriptions

![2bc1e31d73c0da1e8598d766424dd751.png](:/a10e5211520a457fb318c12aec62d1ea)

### Arguments
In REST, there is one place to pass arguments, which is in query params or in the request body. With Graphql, every field and nested object can get its own set of arguments.


- declarative data fetching where a client can specify exactly what data it needs from an API. Instead of multiple endpoints that return fixed data structures, a GraphQL server only exposes a single endpoint and responds with precisely the data a client asked for.
- The `GraphQLSchema` object is the core of a GraphQL server
	- `GraphQLSchema` consists of 2 parts:
		1. schema definition (ie. the structure)
		2. resolver functions that implement the API (ie. the behaviour)
	- Query and Mutate are the rootTypes
- Graphql is completely agnostic to the network layer (usually HTTP) and payload format (normally JSON). In fact, Graphql is not opinionated about the application architecture in general.
- Graphql allows us to make relational queries that allow us to get all the data needed in one trip, instead of having to make multiple calls.

## Between the client and app server
A key thing to understand about GraphQL is that it’s actually agnostic to the way how data is transferred over the network
- ie. it can work on protocols other than HTTP, like websockets

* * *
## Selection Sets
A selection set in Graphql is similar to a result set in SQL, except the selection set is where we specify what we'd like to get back, while a result set in SQL is what we *actually* got back. 

As the name suggests, a selection set is a list of Selections (type Selection)
- a Selection has the following signature:
```
type Selection {
	Field
	FragmentSpread
	InlineFragment
}
```

A selection set might look like:
```
{
	id
	firstName
	lastName
}
```
Some fields may describe more complex data or relationships to other data. To allow us to explore this data, a field itself may contain a selection set.
- To remain unambiguous, the most nested fields must be scalars
