
Since CORS is administered by the server handling the requests, these are response headers. They allow the server to fine-tune its policy toward handling cross-origin requests.

### Access-Control-Allow-Credentials
This header gives the server the power to determine if the browser should expose the response to the front-end Javascript code when the request's [[credentials|protocol.http.requests.prop.credentials]] mode is `include`.