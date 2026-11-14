
Using variables, Directives (`@`) let us dynamically change the structure and shape of our queries.
- This allows us to either `@include` or `@skip` over fields.
- ex. In Never Forget, consider the BrowseNugget screen. Our list component allows us to switch between just showing the nugget name, and showing a summary of the nugget. To determine whether or not we want the summary, we can use a variable:
```gql
query Nugget($details: details) {
	nugget {
		title
		mediaItems @include(if:  $details) {
			title
		}
	}
}
```

Directives shouldn’t affect the value of the results, but do affect which results come back and perhaps how they are executed.

Directives are useful when we find ourselves needing to do string manipulation in order to modify (ie. add/remove fields from) our query structure.

To be Graphql spec-compliant, a server must implement only the `@include` and `@skip` directives. This leaves each server implementation open to extending their own directives.

### Use with inline fragments
Directives can also be applied with inline fragments like so:
```gql
query inlineFragmentNoType($expandedInfo: Boolean) {
  user(handle: "zuck") {
    id
    name
    ... @include(if: $expandedInfo) {
      firstName
      lastName
      birthday
    }
  }
}
```

### Deprecation
Fields in an object may be marked with the `@deprecated` directive like so:
```gql
type ExampleType {
  oldField: String @deprecated
}
```

These fields can still be queried (to prevent breaking changes).
