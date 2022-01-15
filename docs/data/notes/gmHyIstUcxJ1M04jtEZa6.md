
When a browser sends a request to a server, all of the cookies are automatically sent along with that request.

One of the benefits of using cookies versus local storage is that the server receives all of these cookies on initial request. If we are storing a JWT in local storage, then the user will be shown the non-authenticated version of the page initially. The reason is that local storage uses JavaScript, which only gets loaded after the HTML and CSS have been already received. This is a perceivable delay, over just storing the JWT in an http-only cookie.

If you use an HTTP only cookie for authentication, the user's data will be available on the server for the initial render
