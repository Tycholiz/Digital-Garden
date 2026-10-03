
Fragments are the primary unit of composition in Graphql. They...
- must specify the type they apply to
- cannot be specified on any input value (scalar, enumeration, or input object).
- can be specified on object types, interfaces, and unions.

Below, the part after `on` is the type we are selecting from
- so `people` is of type `Person`, and we want the `firstName` and `lastName` fields from `people(id: "7")`
```gql
fragment NameParts on Person {
  firstName
  lastName
}

query GetPerson {
  people(id: "7") {
    ...NameParts
    avatar(size: LARGE)
  }
}
```
## Conditional Fragments
### Type Conditions 
- allow us to conditionally include fields based on their runtime type.

On Facebook, imagine we have 2 different fields that both specify a `count`: User Friends, and Page Likes. We have set up our schema in such a way that we define a `Profile` type, which can be either a `User` or `Page`. 

Now imagine that we want to write a query that gets back both the friends count of Kyle Tycholiz, and it gets back the like count of the Never Forget page. If we want to write this in a single query, we are presented with a problem: while both have a field `count`, the parent field is different, namely `friends` and `likes`. To solve this problem, we must use fragments, which will be applied *depending on* the type of profile that is returned in the query.

When we query for this, the `profiles` root field returns a list where each element could be a `Page` or a `User`.

```gql
{
	profiles(name: ["kyletycholiz", "neverforget"]) {
		name
		...userFragment
		...pageFragment
	}
	
fragment userFragment on User {
	friends {
		count
	}
}

fragment pageFragment on Page {
	likers {
		count
	}
}
}
```

### Inline Fragments
if querying a field that returns an interface or union type, we need to use inline fragments to access data on the underlying concrete type
- These can be used to compose fields in a type-dependent way
    
The above Facebook example can be accomplished using inline fragments. 

The Graphql server will determine whether to return `homeAddress` or `address` at runtime, depending on whether the requested object is a `User` or `Business`

```gql
query Foo {
  profile(id: $id) {
    url
    ... on User {
      homeAddress
    }
    ... on Business {
      address
    }
  }
}
```

#### Practical usage
- Imagine we had an interface `Character` that represents a character from Star Wars:
```gql
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```
- Imagine now we create two types that *implement* this interface:
```gql
type Human implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  starships: [Starship]
  totalCredits: Int
}

type Droid implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  primaryFunction: String
}
```
- As we can see, **Droid** has the field `primaryFunction`, while **Human** does not
    - This means that if we were to make a query that wanted that field back, we would get an error:
```gql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    primaryFunction
  }
}
// PRODUCES ERROR: cannot query field 'primaryFunction' on type Character
```
- To get around this, we need to use an inline fragment:
```gql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
  }
}
```
