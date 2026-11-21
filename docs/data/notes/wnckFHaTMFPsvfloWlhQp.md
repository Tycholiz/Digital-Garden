
## Matching
tldr; be strict with what we request but relaxed with what we expect.
- see [[Postel's Law|general.principles#robustness-principle-postels-law,1]]
- But Pact breaks Postel's law when it comes to request headers

### Request Matching
As a rule of thumb, you generally want to use exact matching when you're setting up the expectations for a request (`upon_receiving(...).with(...)`) because you're under control of the data at this stage
- Note that the request matching does not allow "unexpected" values to be present in JSON request bodies or query strings. (It does however allow extra headers, because it was found that writing expectations that included the headers added by the various frameworks that might be used resulted in tests that were very fiddly to maintain.)

### Response Matching
You want to be as loose as possible with the matching for the response
- This stops the tests being brittle on the provider side. 
- Generally speaking, it doesn't matter what value the provider actually returns during verification, as long as the types match. 
    - When you need certain formats in the values (eg. URLS), you can use `terms`

Be selective with adding matchers in the response.
- ex. the provider might be currently returning a GUID, but would anything in your consumer really break if they returned a different format of string ID? (If it did, that's a nasty code smell!)

### `Pact.term`
- `matcher` will verify that the value matches the given pattern.
    - Sometimes we want to hit an endpoint that uses an id. Since we can't explicitly put an id in the interaction, we can use a *Matcher*, which may take the form of a [[Regex|regex]].
- `generate` allows us to specify a value that will be replayed against the provider as an example of what the real value would look like.
    - this means provider states can still use known values to set up their data, but your Consumer tests can generate data on the fly. 

`Pact.term` can be used for request and response header values, the request query, and inside request and response bodies.

### `Pact.like`
This is for when we don't care what the exact value is at a particular path, you just care that a value is present and that it is of the expected type.
- can wrap a whole object, or a single value:
```js
body: Pact.like(
      name: "Mary",
      age: 73
)
// or
body: {
      name: "Mary",
      age: Pact.like(73)
}
```
