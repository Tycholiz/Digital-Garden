
An access token is used to inform an API that the bearer of the token has been authorized to access the API

The access token doesn't have any concept of sessions. All it cares about is user information. If we are leaving out refresh tokens, then we would only care if the user has an access token or not. If they do, then we trust that they are who they say they are, and we allow access.

The access token contains only the session ID and the user ID, and it expires when the user closes that browser window.
