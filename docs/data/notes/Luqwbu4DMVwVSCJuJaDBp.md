
`context` is an object that gets passed through the resolver chain that each resolver can read from and write to (basically a means for resolvers to communicate and share information).
- If we were using Apollo Server, we would set the context when initializing a new `ApolloServer`.

holds info like... 
- currently logged in user, 
- current access to the database (which includes postgres user) etc.
- access token
- correlationId
- [[data sources|general.patterns.data-source]]
    - `dataSource` is part of the constructor for the `ApolloServer` class. The `dataSource` instances are used by the resolvers via the `context` object.

Therefore, we can use the context to provide access to the database
