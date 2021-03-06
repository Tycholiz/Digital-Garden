
Introspection is about the links between types and scalars
The Graphql server is what supports introspection over its schema.
Types and fields required by the Graphql introspection systema are prefixed with `__`

Determining which types are available
```
{
  __schema {
    types {
      name
    }
  }
}
```

## Introspection Types
`__Type` is at the core of the type introspection system. It represents scalars, interfaces, object types, unions, enums in the system.
- All types in the introspection system provide a `description` field to allow type designers (ie. us, the developer) to publish documentation about a type.

### __Field
Represents each field in an Object or Interface type.


### __InputValue
Represents field and directive arguments as well as the inputFields of an input object.

Has fields:
- `name`— returns string
- `description`— returns string or null
- `type`— returns `__Type`, representing the type this input value expects
- `defaultValue`

## Introspection Queries
The schema introspection system is accessible from the meta‐fields `__schema` and `__type` which are accessible from the type of the root of a query operation.

Can be of 3 types:
```
__schema: __Schema!
__type(name: String!): __Type
__typename: String!
```

These fields are implicit and do not appear in the fields list in the root type of the query operation.

The schema of the GraphQL schema introspection system:

```
type __Schema {
  types: [__Type!]!
  queryType: __Type!
  mutationType: __Type
  subscriptionType: __Type
  directives: [__Directive!]!
}

type __Type {
  kind: __TypeKind!
  name: String
  description: String

  # OBJECT and INTERFACE only
  fields(includeDeprecated: Boolean = false): [__Field!]

  # OBJECT only
  interfaces: [__Type!]

  # INTERFACE and UNION only
  possibleTypes: [__Type!]

  # ENUM only
  enumValues(includeDeprecated: Boolean = false): [__EnumValue!]

  # INPUT_OBJECT only
  inputFields: [__InputValue!]

  # NON_NULL and LIST only
  ofType: __Type
}

type __Field {
  name: String!
  description: String
  args: [__InputValue!]!
  type: __Type!
  isDeprecated: Boolean!
  deprecationReason: String
}

type __InputValue {
  name: String!
  description: String
  type: __Type!
  defaultValue: String
}

type __EnumValue {
  name: String!
  description: String
  isDeprecated: Boolean!
  deprecationReason: String
}

enum __TypeKind {
  SCALAR
  OBJECT
  INTERFACE
  UNION
  ENUM
  INPUT_OBJECT
  LIST
  NON_NULL
}

type __Directive {
  name: String!
  description: String
  locations: [__DirectiveLocation!]!
  args: [__InputValue!]!
}

enum __DirectiveLocation {
  QUERY
  MUTATION
  SUBSCRIPTION
  FIELD
  FRAGMENT_DEFINITION
  FRAGMENT_SPREAD
  INLINE_FRAGMENT
  SCHEMA
  SCALAR
  OBJECT
  FIELD_DEFINITION
  ARGUMENT_DEFINITION
  INTERFACE
  UNION
  ENUM
  ENUM_VALUE
  INPUT_OBJECT
  INPUT_FIELD_DEFINITION
}
```

### __schema
The most important field (query) in graphql, as it allows us to fetch the whole schema. It is also the primary source for Graphiql. The name for this query is `__schema`, and its Schema Definition Language (SDL) is:
```
type __Schema {
  types: [__Type!]!
  queryType: __Type!
  mutationType: __Type
  subscriptionType: __Type
  directives: [__Directive!]!
}
```

As we can see, when we query `__schema`, we get some information about the whole schema, including which operation types we have, and which types and directives we have. The result of that query might look like this:
```
{
  __schema {
    directives {
      name
      description
    }
    subscriptionType {
      name
      description
    }
    types {
      name
      description
    }
    queryType {
      name
      description
    }
    mutationType {
      name
      description
    }
    queryType {
      name
      description
    }
  }
}
```

### __type
Allows us to query for information about the exact type we are interested in. All we need to do is pass in the type as an argument like so:
```
query introspectionUserType {
  __type(name: "User") {
	name
	kind
	fields {
		name
		type {
			name
		}
	}
  }
}
```
This might return:
```
{
  "__type": {
    "name": "User",
	"kind": "OBJECT"
    "fields": [
      {
        "name": "id",
        "type": { "name": "String" }
      },
      {
        "name": "name",
        "type": { "name": "String" }
      },
      {
        "name": "birthday",
        "type": { "name": "Date" }
      },
    ]
  }
}
```

When the `name` field is `null`, it is because it is a wrapper type (ID, List). If we query for the `ofType` on these fields, we can see what the "wrapped" type is
- ex. a list is a wrapper type, because each item in the list has its own type. The List is just a type that wraps them altogether. The same can be said for Non-null and ID.

### __typename
Offers type name introspection. `__typename` is a meta-field that allows us to get the type name at any point within a query. It is available through all types when querying.

Apollo client makes use of `__typename` to construct the apollo cache. It uniquely identifies an item by `id + __typename`
