
There is a distinction between Graphql Documents and Operations:
- An operation is a [[query|graphql.documents.queries]], [[mutation|graphql.documents.mutations]], or [[subscription|graphql.documents.subscriptions]]
- A document is one or more operation (including any [[fragments|graphql.documents.fragments]]) stringified in a request body.

`query` types and `mutation` types are the entry point to a graphql query
- It's important to remember that other than the special status of being the "entry point" into the schema, the Query and Mutation types are the same as any other GraphQL object type, and their fields work exactly the same way.

Every GraphQL query has the shape of a tree — i.e. it is never circular. 

Scalars are primitive values in GraphQL. If we consider a graphql query as a hierarchical graph, the leaves would be the primitives

GraphQL requires that you construct your queries in a way that only returns concrete data
- Each field has to ultimately resolve to one or more scalars (or enums). That means you cannot just request a field that resolves to a type without also indicating which fields of that type you want to get back (hence why `graphiql` will auto-complete for us).

two operations that occur in GraphQL servers are:
1. **result coercion**: upholding the contract of a type which we receive from the server (basically upholding the primitive values or object type)
	- The type system knows what to expect and will convert the values returned by a resolver function into something that upholds the API contract
2. **input coercion**: upholding the contract of a type for input arguments that we pass into the GraphQL query or mutation
	- if we pass in `5` for the `id` field, the type will be parsed into a string as `"5"`

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
