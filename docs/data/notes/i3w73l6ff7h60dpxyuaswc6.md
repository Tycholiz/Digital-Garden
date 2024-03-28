
An ETag (entity tag) is an HTTP response header that specifies a specific version of a resource.

The ETag enables servers to inform clients when requested data has not changed. When a client makes a request for a resource, it passes along an ETag (which it got from the previous fetch), and instead of the server just sending back all of the data upon request, it can match the passed ETag with the ETag of the reource and inform the client that the data requested has not changed since the previous fetch.

Any time a resource is changed, a new ETag value must be generated. 

### `If-None-Match` Header
For GET and HEAD methods, the server will return the requested resource and 200 status, only if it doesn't have an ETag matching the one passed by the client.

### `If-Match` Header
For GET and HEAD methods, the server will only return the requested resource (or upload resource for PUT) if the passed ETag matches one of the ETags from the server.