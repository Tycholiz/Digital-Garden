
### httpOnly Cookie
a cookie that is only sent in the http requests to the server.
- Therefore, never accessible to the client-side javascript (making it immune to XSS attacks.)

Cookies and local storage can be accessed by anything client-side. This includes things like extensions. This makes storing sensitive information in a cookie a security risk. However, an HTTP-only cookie is different, in that its contents can only be set and read server side. This is why it is OK to store JWTs in an HTTP-only cookie.
The cookie is set via the response payload from the server

The clients only real job with an HTTP-only cookie is to store the cookie itself
