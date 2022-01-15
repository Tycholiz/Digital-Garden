
The Refresh Token contains basically only the session ID which can be used to look up in the database to see if it’s an active session or not. If it is, then it can generate a new [[access token|auth.jwt.access-token]] and that can be sent to the user
- A refresh token is a special token that is used to generate additional jwt tokens. This allows you to have short-lived access tokens (JWTs) without having to collect credentials every time one expires. The server sends the client this token alongside the access (JWT) and/or ID tokens as part of a user's initial authentication flow
	- The refresh token should be saved in the database in the relevant row of the user's table.
		- Therefore, we can handle the renewing login with Postgres.
	- the refresh token can be sent from the server to the client as an `HTTPOnly` cookie
- A refresh token is not capable of authenticating a user on its own— its only use is to look up active sessions. If it finds one, then the refresh token can be used to generate a new access token.
- Every time a new access token is generated, a refresh token should be generated as well
- a Refresh token is what allows us to login to a website, close the browser, and still be logged in upon reopening it.
	- When a new session starts (reopening the browser tab), the app is able to see that there is no JWT in memory, so it triggers a *silent refresh*
- Imagine we set a jwt to have a lifetime of 15 minutes. Without the refresh token, this means that the server would send back an http 401: unauthorized every 15 minutes (probably at which point your app will log the user out and display the login screen)
- A refresh token has 2 properties:
	1. It can be used to make an API call (say, `/refresh_token`) to fetch a new JWT token before the previous JWT expires.
	2. It can be safely persisted across sessions on the client!

### Why use refresh tokens?
Using refresh tokens in conjunction with access tokens helps us alleviate the problem of stale tokens. e
