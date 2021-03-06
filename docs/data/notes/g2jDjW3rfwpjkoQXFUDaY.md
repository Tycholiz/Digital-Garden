
The frontend aims to be stateful (that is, keep track of the state between requests). If the frontend wasn’t stateful, you would have to log in every time you navigated to a new page.
The backend, however, aims to be stateless. This means that the state must be provided on every new invocation. For instance, the API does not keep track of whether you are logged in or not. It determines your authentication state by reading the token in your API request.
- If you used a Redux store on your Node.js server, the state would be cleared every time the node process stops
- It becomes even more involved when you consider scaling. If you were to scale your application horizontally by adding more servers, you’d have multiple Node processes running concurrently, and each would have their own version of the state. This means that two identical requests to your backend at the same moment could easily get two different responses.
