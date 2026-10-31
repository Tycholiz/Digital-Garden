
The input to the `fetch` function is a request, which consists of...
- a [[method|protocol.http.methods]]
- a [[URL|protocol.http.URL]]
- a list of headers
- a body
- ...and many [others](https://fetch.spec.whatwg.org/#requests), which are mostly implicitly set.

The response of the `fetch` function is a response, which consists of...
- a list of headers
- a body
- status
- ...and many [others](https://fetch.spec.whatwg.org/#responses), which are mostly implicitly set.

A response evolves over time. That is, not all its fields are available straight away.

### Request/Response Body
A body consists of:
- A [[stream|web.api.streams]] (a `ReadableStream` object).
- A source (null, a byte sequence, a `Blob` object, or a `FormData` object), initially null.
- A length (null or an integer), initially null.

### Credentials
For cross-origin requests, `credentials` allows us (as the client) to specify whether or not credentials should be sent along for the ride in HTTP requests.
- Therefore this gets set on the client.

Credentials allow the server to maintain state about a particular user across multiple requests. 
- ex. it's how Twitter shows you your feed, it's how your bank shows you your accounts.

Credentials are [[cookies|browser.cookies]], authorization headers, or TLS client certificates (not to be confused with server certificates). 
- Basically, like the email/password credentials we are most familiar with, credentials verify identity and are a way to establish trust.

Both the client and the server must indicate that they’re opting into including credentials.