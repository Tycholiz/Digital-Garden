
In order to interpret and understand these unique and complex requests, GraphQL employs the use of an Abstract Syntax Tree (AST) to structure the incoming requests.
- This makes it easier for the back-end gurus & frameworks to parse and construct the required response.

As a consequence of Graphql being data-layer-independent, libraries such as graphql-js and graphql-tools exist to abstract away the heavy lifting, removing our need to worry about having to interact with the underlying AST.
- Sometimes however we must be able to perform our own custom operations based on the specifics of the user's request.

When a user makes a request, GraphQL combines the query document that the user requested with the schema definition that we defined for our resolver in the form of an AST. This AST is used to determine which fields were requested, what arguments were included and much more.

### Rugby Player Example
Suppose we wanted to expose a list of rugby players on our Graphql backend. We make a schema definition which models a new Graphql type `RugbyPlayer`, which has 2 fields: `full_name` and `club`:
```js
export const RugbyPlayer = new gql.GraphQLObjectType({
    name: 'RugbyPlayer',
    fields: {
        full_name: {
            type: gql.GraphQLString,
        },
        club: {
            type: new gql.GraphQLObjectType({
                name: 'Club',
                fields: {
                    name: {
                        type: gql.GraphQLString,
                    },
                },
            }),
        },
    },
})
```
Imagine now that a client makes a request for that list of players. From the Graphql server's perspective, the schema knows nothing about the query document (ie. the request). Therefore, it has to parse the query document into AST format (ie. heavily nested objects) before it can traverse it. Now, it can perform any necessary validation, like throwing errors for having the wrong types for fields or arguments.
- Following this validation, schema types are mapped onto the respective branches of the AST to provide a richer set of metadata.

### What we can do with the AST
By understanding the AST, we can craft custom directives and optimise user requests
- We do this by breaking the traditional Graphql lifecycle and inercepting a request before it gets passed onto another library to generate a response.

By traversing and augmenting the AST we can implement:
- Schema stitching
- Custom directives
- Enriched queries
- Layered Abstraction
- More backend magic!

In a step to give more control to the front-end client, we often find it useful to implement a range of directives that can transform or filter out fields specified in the AST.
- ex. In the Rugby example, imagine we want to solve this problem: "Rugby player's names are being mispronounced too often".
	- What we can do is implement a Graphql directive called `@pronounce`, which can be added onto the `full_name` field when we are querying our `RugbyPlayers` type:
```
query {
  RugbyPlayers {
    full_name @pronounce
  }
}
```
libraries like `graphql-js` can provide methods such as `gql.GraphQLDirective` that allow us to add these directives.

Whenever we see code like this:
```
const Type = gql`
  type ${nameOfType} {
    str: String
    int: Int
  }
`;
```

We can understand `gql` as a function that transforms our human-readable graphql schema language into an AST that the application can understand.
