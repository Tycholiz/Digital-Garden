
The goal is to have a single source of truth in Graphql+Typescript projects
- When we work with Graphql and Typescript, there is a large amount of duplication of data needed. First, we need to define the Graphql types. Then we need to make a variation of that same data for Typescript (interfaces and types). Then we potentially need to update ORMs and Schemas. All from a single alteration of an object model.
- The idea of the library is to automatically create GraphQL schema definition from TypeScript's classes

Type-graphql provides a `buildSchema` function which takes in all the resolvers we've made and spits out a Graphql schema.

To avoid the need of schema definition files and interfaces describing the schema, Type-graphql use a bit of reflection magic and decorators.

# API
## Types and Fields
### `@ObjectType()`
- Mark the class that follows as the type known from GraphQL SDL
```ts
@ObjectType()
class Recipe {
  @Field()
  id: string;
}
```

- [source](https://typegraphql.com/docs/0.17.0/types-and-fields.html)

### `@Field()`
- Map the class property that follows as a field on the Graphql type
## Resolvers
spec: distinct from [[Graphql Resolvers|graphql.server.resolver]]

Resolvers enable us to perform CRUD actions (queries and mutations).
- It could be thought of as a Controller from classic REST frameworks.

### `@Resolver()`

### `@Query()`
`@Query(returns => [Recipe])` means that the query resolves to an array of `Recipe` objects
