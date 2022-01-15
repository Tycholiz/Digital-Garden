
HTTP doesn't allow for writing, modifying, moving, etc of a file; it only allows for querying that file. WebDAV enhances HTTP to allow writing.


- an extension of HTTP
	- Therefore, gets all of the benefits that HTTP offers, such as encryption, if HTTPS
	- Also can use HTTP tools like cURL
- Since it is an extension of HTTP, it gets access to HTTP verbs. Additionally, it extends these base verbs, giving additional functionality
	- ex. COPY, MOVE, MKCOL (make collection, aka directory)
- a protocol that allows us to create, update, and move documents on a server.
	- these are known as *remote web content authoring operations*
- WebDAV provides a coherent set of methods and headers, and has a system like Express that involves request and response objects
- ex. perform CRUD operations on information about Web pages, such as their authors, creation dates, etc.
- The WebDAV protocol enables a webserver to behave like a fileserver
