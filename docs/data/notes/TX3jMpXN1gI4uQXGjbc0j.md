
# Operation Types (ie. Queries and Mutations)
- `query` types and `mutation` types are the entry point to a graphql query
	- It's important to remember that other than the special status of being the "entry point" into the schema, the Query and Mutation types are the same as any other GraphQL object type, and their fields work exactly the same way.

Scalars are primitive values in GraphQL. If we consider a graphql query as a hierarchical graph, the leaves would be the primitives

GraphQL requires that you construct your queries in a way that only returns concrete data
- Each field has to ultimately resolve to one or more scalars (or enums). That means you cannot just request a field that resolves to a type without also indicating which fields of that type you want to get back (hence why `graphiql` will auto-complete for us).

two operations that occur in GraphQL servers are:
1. **result coercion**: upholding the contract of a type which we receive from the server (basically upholding the primitive values or object type)
	- The type system knows what to expect and will convert the values returned by a resolver function into something that upholds the API contract
2. **input coercion**: upholding the contract of a type for input arguments that we pass into the GraphQL query or mutation
	- if we pass in `5` for the `id` field, the type will be parsed into a string as `"5"`
