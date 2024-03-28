
Resolvers specify how the types and fields in the schema are connected to various backends, making them essentially mini-routers. 
- Through these functions, you are able to answer questions such as “How do I get the data regarding Course Authors?” and “Which backend do I need to call with what arguments to get the data regarding Courses?”.

Each graphql type needs a resolver. The resolver exists to resolve a type from some data layer (field resolvers are the same thing; they just resolve fields on a type)

If we were converting from a REST API with 20 different endpoints, then we would end up with close to 20 resolvers
- this mapping is not absolute, but is a good rough guide. For example, maybe we want to implement a resolver `createOrUpdateBook`. RESTful best practice would have us splitting the creation and updating of a book exist as 2 different endpoints, but this would be fine to combine as a single [[mutation|graphql.documents.mutations]]. In this case, we would implement some logic in the resolver "if an `id` is passed as input to the mutation, update the book. Otherwise, create it".

A Resolver is a collection of functions whose responsibility is sourcing the data for a particular field. They are responsible for generating a response to the Graphql query.
- the function is mapped to a schema. In other words, each type in the schema has a corresponding resolver
- The schema (made up of queries and mutations) says "here's what you can look at", and the resolver says "here's how you get it"
	- If we had a SQL database, the resolver could be configured to call some REST endpoint, and that REST endpoint would ultimately execute the SQL query.
- The resolvers map to your schema and are what GraphQL actually executes to retrieve each piece of data. The resolvers are like controllers in a regular REST API.
- a resolver receives 4 args:
	1. obj
	2. args
	3. context
	4. info

### `obj`
the previous object in the graphql "tree" (with the root being `query` or `mutate`). It contains the result returned from the resolver on the parent field.
- sometimes aka `root`
- when we are making a resolver function on the root Query type, we probably won't need to use `obj`.
- All Graphql has to do in order to resolve a query is call the resolvers on the query's fields. This is being done level by level (in other words, from most outdented to most indented; ltr).
- `obj` argument in each resolver call is simply the result of the previous call
	- ex. we are querying `getNuggetById`. `obj` is the `query` type at this point. When the backend receives that request, the resolver executes a db query and returns to us `{ id: 1, title: 'first nugget' }`. With the first field resolved, the value of `obj` on the second iteration is the `nugget` type, since that is what was returned by the first iteration. This is precicely the reason why we don't have to explicitly write resolvers for every single field.
	- In fact, if we were to console.log `obj`, on the second iteration we'd have the nugget object.
```gql
query {
	nugget(id: $id) {
		id
		title
	}
}
```

### `args`
The arguments provided to the field in the GraphQL query

### `context`
![[graphql.server.resolver.context]]

### `info`
holds field-specific information relevant to the current query as well as the schema details
- The way you form relationships is by defining custom types in your resolvers
- In its most basic form, a GraphQL server will have one resolver function per field in its schema
	- Each resolver knows how to fetch the data for its field

## How GraphQL resolves fields
While you certainly can write a resolver for every field in your schema, it's often not necessary because the [[graphql.server]] uses a default resolver when you don't provide one.
- in most cases, the GraphQL library will just omit simple resolvers and will just assume that if a resolver isn't provided for a field, that a property of the same name should be read and returned.
- what the default resolver does is simple: it looks at the value the parent field resolved to and if that value is a JavaScript object, it looks for a property on that Object with the same name as the field being resolved. If it finds that property, it resolves to the value of that property. Otherwise, it resolves to null.
	- This process is the reason why deeply nested queries are more computationally expensive.
- GraphQL queries always end at scalar values.

# E Resources
[Good breakdown of how schema/resolvers work](https://www.prisma.io/blog/graphql-server-basics-the-schema-ac5e2950214e)
