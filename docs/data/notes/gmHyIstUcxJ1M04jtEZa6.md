
When a browser sends a request to a server, all of the cookies are automatically sent along with that request.

One of the benefits of using cookies versus local storage is that the server receives all of these cookies on initial request. If we are storing a JWT in local storage, then the user will be shown the non-authenticated version of the page initially. The reason is that local storage uses JavaScript, which only gets loaded after the HTML and CSS have been already received. This is a perceivable delay, over just storing the JWT in an http-only cookie.

If you use an HTTP only cookie for authentication, the user's data will be available on the server for the initial render

### httpOnly Cookie
a cookie that is only sent in the http requests to the server.
- Therefore, never accessible to the client-side javascript (making it immune to XSS attacks.)

Cookies and local storage can be accessed by anything client-side. This includes things like extensions. This makes storing sensitive information in a cookie a security risk. However, an HTTP-only cookie is different, in that its contents can only be set and read server side. This is why it is OK to store JWTs in an HTTP-only cookie.
The cookie is set via the response payload from the server

The clients only real job with an HTTP-only cookie is to store the cookie itself
