
An access token is used to inform an API that the bearer of the token has been authorized to access the API

If using a JWT without sessions, do not store the token in localStorage. You should keep it in state.

When the server receives the access token JWT, it extracts the access token from the Authorization header. It then validates the access token by checking its signature using the secret key (this ensures the token was issued by your server), then
checking whether the token has expired (JWT includes an exp claim).

The access token doesn't have any concept of sessions. All it cares about is user information. If we are leaving out refresh tokens, then we would only care if the user has an access token or not. If they do, then we trust that they are who they say they are, and we allow access.

The access token contains only the session ID and the user ID, and it expires when the user closes that browser window.

Access tokens are typically short lived (compared to the typically longer-lived [[auth.tokens.refresh]])
- the benefit here is that if the access token is intercepted, the damage can be limited, since the token will expire imminently.