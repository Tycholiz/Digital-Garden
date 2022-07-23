
## GET
- GET requests can be cached
- GET requests remain in the browser history
- GET requests can be bookmarked
- GET requests should never be used when dealing with sensitive data
- GET requests have length restrictions
- GET requests should be used only to retrieve data

### Return codes
- 200 (OK)
- 400 (Bad Request)
- 404 (Not found)

## POST
Use POST when you want to create resources *without* knowing the URI for the new resource
- however, a POST does not necessarily have to result in a resource that can be identified by a URI.

- POST requests are never cached
- POST requests do not remain in the browser history
- POST requests cannot be bookmarked
- POST requests have no restrictions on data length
- POST requests are not idempotent

### Status codes
- 200 (OK) - more than 1 resource created
- 201 (Created) - only 1 resource created
- 204 (No Content) - no new resource created (or if a resource was created, but the response doesn't include an entity that describes the result)
- 400 (Bad Request)

## PUT
For PUT requests the client *needs* to know the exact URI of the resource.
- ex. We cannot send a PUT request to `/projects` and expect a new resource to be created at `/projects/123`
- this doesn't mean we can't create new resources with PUT; but it does mean the client needs to know how to generate the URI/ID of the new resource.
    - In cases where this ID is knowable by the client, PUT should actually be preferred over POST, since PUT is idempotent.
- Therefore, PUT can be thought of as "create or update" resource.

PUT should replace the existing resource with the new one. This means we cannot do partial updates.
- if we want to mimic a partial update, then we'd have to send the whole resource (with the modified field).

### Status codes
- 201 (Created) - a new resource is created
- 200 (OK) - an existing resource is modified
- 204 (No Content) - an existing resource is modified, but the entity isn't included in the response

## PATCH
PATCH allows us to perform partial modifications to an existing resource.

PATCH requests are neither safe nor idempotent

### Status codes
- 200 (OK)
- 400 (Bad Request) 
- 404 (Not found)
- 422 (Unprocessable Entity)

## Delete

### Status codes
- 204 (No Content)
- 400 (Bad Request)
- 404 (Not found)

* * *

## Properties of HTTP methods
### Safe
HTTP methods are considered safe if they do not alter the server state, so safe methods can only be used for read-only operations.

Safe HTTP methods are GET, HEAD, OPTIONS and TRACE

### Idempotent
[[main note|general.terms.idempotent]]

The idempotent methods are GET, HEAD, OPTIONS, TRACE, PUT and DELETE.

All safe HTTP methods are idempotent but PUT and DELETE are idempotent but not safe.