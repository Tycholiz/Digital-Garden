
A GraphQL server essentially takes in your API and exposes your GraphQL API via an endpoint. It has two core parts:
- A Schema, which includes type definitions.
- Resolve functions, which hold all functions that define how to get the data

# Type Coercion
when preparing a field of a given scalar type, a GraphQL server must uphold the contract the scalar type describes, either by: 
- coercing the value
- producing a field error if a value cannot be coerced or if coercion may result in data loss.

A GraphQL server may decide to allow coercing different internal types to the expected return type. Unless the coercion is sensical (ie. no information is lost), the Graphql server will raise a field error.
- For example when coercing a field of type `Int`, a boolean `true` value may produce `1`. Alternatively, a string value "123" may be parsed as base‐10 123.

The Graphql server will also coerce input values it receives as arguments to fields, as long as those arguments are a scalar type.
- For example, if we pass a string `"4"` or int `4` to an ID input type, the value should be coerced to an ID format (as expected by the Graphql server)

# Examples
`express-graphql`, `apollo-server`, `graphql-yoga`
	- In Express, these are nothing but middleware functions that act as glue between the request and GraphQL engine provided by graphql.js (the tool that provides functionality for resolving queries)
- express-graphql has 2 responsibilities:
	1. Ensure that the GraphQL query (or mutation) contained in the body of an incoming POST request can be executed by GraphQL.js.
	2. Attach the result of the execution to the response object so it can be returned to the client.
- apollo-server is more generic than express-graphql, so it works in other frameworks, in addition to express.
	- it can also be configured to work as FaaS, like AWS Lambda
- graphql-yoga
	- like create-react-app for GraphQL servers
