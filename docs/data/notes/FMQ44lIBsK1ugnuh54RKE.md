
# Schema
- specifies the capabilities of the API and defines how clients can request the data. It is often seen as a contract between the source of data (ie. server) and its destination (ie. client), and it defines what data can be queried and how it ought to be queried
- the schema gives the app's backend the ability to say "here's the data that I want to make available to the client"
- when queries come in, they are validated and executed against the schema
- Our schema, along with the requested query, defines the "shape" of the data object in the response returned by our endpoint
	- By shape, we mean what properties objects have, and whether those properties' values' are scalar values, other objects, or arrays of objects or scalars
- Prefer building a GraphQL schema that describes how clients use the data

The schema is a mode of the data that can be retrieved through the Graphql server. It specifies:
- It specifies what queries clients are allowed to make
- what types (scalar, object, query, mutation) of data can be fetched from the server
- what the relationships between these types are.

The Schema says "Here's the data you can look at". If we were to imagine our database as a complex graph, then the schema would be the graph. The schema defines how we can query and alter the data of the underlying database.

- ex. a simple user and post app `schema.gql`:
```
// schema may be implicitly defined
schema {
	query: Query
	mutation: Mutation
}

type Query {
	getUser(uuid: String!): User
}

type Mutation {
	createUser(input: UserInput!): User
}

type User {
	UUID: String
	Name: String
	Posts: [Post]
}

type Post {
	UUID: String
	Text: String
}

input UserInput {
	Name: String
	Posts: [PostInput]
}

input PostInput{
	Text: String
}

```

A field does not become queryable until it exists on the `Query` type:
```
type Query {
	myName: String
}
```

## Descriptions
Descriptions are a first-class way of documentation in Graphql. To implement, we only need to include comments before the definition we are describing. There are 2 different ways to add documentation: Comment blocks for constructs, and inline comments for fields:
```graphql
"""
A simple GraphQL schema which is well described.
"""
type Query {
  """
  Translates a string from a given language into a different language.
  """
  translate(
    "The original language that `text` is provided in."
    fromLanguage: Language

    "The translated language to be returned."
    toLanguage: Language

    "The text to be translated."
    text: String
  ): String
}

"""
The set of languages supported by `translate`.
"""
enum Language {
  "English"
  EN

  "French"
  FR

  "Chinese"
  CH
}
```


