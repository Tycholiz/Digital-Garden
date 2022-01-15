
# Types
In the same way a schema defines the shape of the total response, the type of an individual field defines the shape of that field's value.
- The shape of the data we return in our resolver must likewise match this expected shape. When it doesn't, we frequently end up with unexpected nulls in our response.
GraphQL is based on its type system, and depending on what type the GraphQL server receives, it will determine what it should do next.
- If we are trying to execute a query to get all nuggets, then the GraphQL server will see we are querying an object (nuggets). Since it is an object (and therefore not a scalar, which would end the execution cycle), GraphQL server knows that it needs to "fetch" those fields on the object. It then checks the type of each field. If they are non-scalar, then it checks for more data. This continues until every field is a scalar type. 

# User-defined Types
In GraphQL we deal with 8 different types:
- scalars, enums, lists, objects, input objects, interfaces, unions, non-null

## Scalar
Scalar types are:
1. Int
2. Float
3. String
4. Boolean
5. ID

A GraphQL object type has a name and fields, but at some point those fields have to resolve to some concrete data. These basic types of data are called scalars, and they are the "atomic type" of GraphQL
- At each type, graphql will keep resolving until it gets to the scalar

Scalar types resolve to a scalar object. By definition, they cannot have sub-selections (fields) in the query.

All Graphql scalars are representable as strings

Graphql provides built-in scalars, but type systems can add additional scalars
- ex. Imagine making a scalar called `Time`, which we define as conforming to ISO-8601. When we query a field of type `Time`, we can rely on the ability to parse the result with an ISO-8601 parser. On the client, we can use a primitive (such as Date) to represent the value.
- ex. Imagine a scalar called `Url`. It would still be serialized as a string, but would be guaranteed by the server to be a valid URL.

### ID
ID is a wrapped type of kind `NON_NULL`

## List
Represents are sequence of values in Graphql.
- A list type is a *type modifier*, meaning it wraps another type, found in the `ofType` field. This field defines the type of each item in the list.

We can request paginated data from list fields with the `first` argument

## Object
While scalars represents the leaf values of a query hierarchy, objects represent the intermediate levels.

Represent concrete instantiations of sets of fields. the introspection types (e.g. `__type`, `__field`, etc) are examples of objects.

## Input Object
A composite type used for inputs into queries, defined as a list of named input values.
- often, an update and create operation on a db record will take in the same inputs (ex. title, mediaitems...).
- an input type is simply a type that includes all of these same inputs so we can reuse it in multiple places.
- input types can't have fields that are other objects, only basic scalar types, list types, and other input types.
```
type nugget {
	id: id!
	title: string
	mediaitems: json
}

input nuggetinput {
  title: string
  mediaitems: json (?)
}

// then...

type mutation {
  createnugget(input: nuggetinput): nugget
  updatenugget(id: id!, input: nuggetinput): nugget
}

```

## Null
a null result on a Non-Null type bubbles up to the next nullable parent. If this bubbling never stops because everything is of Non-Null type, then the root data field is null.
- in other words, if we are trying to query a field that is `NONNullable` and the result happens to be `null`, that `null` will bubble, and the parent "object" will result in null. If however the field we are querying is `nullable` and the result happens to be `null`, it will remain at that field and not bubble
- When fields are nullable, you still get partial data: even if you don't have the name, you can still show the age and other fields. When you mark fields as Non-Null, as in the second example, you forfeit your right to partial data.
[source](http://spec.graphql.org/June2018/#sec-Errors)

## Interface
Similar to how fragments allow us to DRY our queries, interfaces can DRY our type definitions.

Interfaces represent a list of named fields and their arguments. These interfaces can then be implemented by objects.

## Union
Unions represent an object that could be one of many specified types. However, there is no guarantee that any of the fields on each of those types will be provided (whereas interfaces guarantee that a field will be available)

Unlike interfaces, Unions do not implement fields of their own

They differ from interfaces in that Object types declare what interfaces they implement, but they are not aware of what unions contain them.

```
union SearchResult = Photo | Person

type Person {
  name: String
  age: Int
}

type Photo {
  height: Int
  width: Int
}

type SearchQuery {
  firstSearchResult: SearchResult
}
```

## Non-null
Graphql fields are nullable, making `null` a valid response for field type.
- Like lists, a non-null type is a type modifier (it wraps another type instance in the `ofType` field).
