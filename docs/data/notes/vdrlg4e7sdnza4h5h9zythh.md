
REST is a set of architectural constraints for an API.

REST is typically built on top of [[protocol.http]]

When a client makes a request to a REST API, the request serves as a representation of the state of the resource.

The API is client-facing. Therefore, any questions about what an API endpoint does should ultimately fall back to the user's expectations. 
- ex. soft-delete is a common implementation, where a column on a database item called `isDeleted` is simply switched to `true`, rather than actually removing an item from a table. From an API design standpoint, the question then becomes "which [[http method|protocol.http.methods]] is this, a DELETE request, or PATCH request?". If a DELETE request is for *deleting resources*, then surely we'd want to use a PATCH request. However, the logic is that the API hides implementation details behind its interface. The implementation of a soft-delete is of no importance to the client using the API, therefore none of the interface should reflect any kind of implementation details. A DELETE http request is the right choice here because of this.

## Terminology
- *resource* - any piece of information that can be accessed from the API
    - Each resource has a unique name, called the *resource identifier*.