
The gql helper is responsible for turning the human-readable GraphQL schema language you write into an abstract syntax tree ([[AST|graphql.ast]]) that the application can understand.

the `gql` helper differs slightly from the one from `graphql-tag`, namely in how the placeholders work.
- This `gql` function is designed to work with PostGraphile's inflection system, so you can embed strings directly
- We can also embed other gql tags directly

```js
const nameOfType = "MyType"; // Or use the inflection system to generate a type

// This tag interpolates the string `nameOfType` to allow dynamic naming of the
// type.
const Type = gql`
  type ${nameOfType} {
    str: String
    int: Int
  }
`;

// This tag interpolates the entire definition in `Type` above.
const typeDefs = gql`
  ${Type}

  extend type Query {
    fieldName: Type
  }
`;
```
