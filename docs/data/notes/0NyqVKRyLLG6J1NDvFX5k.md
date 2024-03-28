
### General flow
1. When user logs in with email and password, 
2. The authentication server verifies the identity of the user
3. If successful, the session server creates an entry in its database (ie. a session) for the user, associating the session with the JWT [[access token|auth.tokens.access]] and defining its TTL (time to live).
4. The server then responds to the client, providing it with the sessionId. 
5. The client then stores this sessionId somehow (e.g. in a [[cookie|browser.cookies]]). 
6. The client can now send info in that cookie with each request to the server, meaning that any server can check to see if there is an active session or not.

![](/assets/images/2022-12-14-13-09-43.png)

The session server database (which stores the session) will associate the session with the refresh and access tokens

Sessions are stateful (unlike [[JWTs|auth.tokens.jwt]], which are stateless)
- This means that one of the communicating parties must keep track of (store) information about the current state, and save information about session history.

Sessions piggyback off of stateless HTTP requests to make a stateful connection.
- anal: the way TCP piggybacks on top of IP

When there is an active session, the server responds to the request with an [[ETag|protocol.http.etag]] (an id corresponding to a particular "version" of data). If the data arrives and it is identical to the ETag, then we know it is the same as the cached version.

In a session-based workflow, a session is active until the user hits the logout button (or remains logged out for a longer period of time)

We can store additional data into the session as needed, such as the user’s set of permissions or anything else that is potentially useful.

sessions are not an option for *mobile apps*.

## Considerations
Since sessions are stored server-side, administrators of the app retain full power and can rescind privileges of a session simply by destroying it and forcing the user to log in again.

Session-based workflows introduce scalability concerns due to the inherent bottleneck behind which your data sits.
- when using sessions, the resource servers must verify the session information with each request. It must therefore make its own request to the Session server, which will verify the session information.
  - spec: This problem could be solved by using something like [[apollo.federation]], passing a token with the graphql request and having the gateway server perform the session lookup for you

Broadly speaking, session-based authentication methods are better suited for user-to-server connections, while [[token|auth.tokens]]-based methods are better suited for server-to-server connections.

### Sessions vs Tokens
- The session authentication method is based on the concept of the ID being shared with the client through a cookie file, while the rest of the details are on the session file, stored on the server.
- The token-based authentication method is based on the concept that possessing a token is the only thing that a user needs to have their requests authorized by the server, which must only verify a signature. The token is secure to use because it cannot be tampered with.
- Both methods have inherent vulnerabilities that can be most easily resolved with different workarounds. In the end, it is up to the individual needs of the development team and their applications.

## Persistent Session Store
a place to store all session-related data (but persistent so it doesn't get erased when the user closes the tab or app)
- session store is like the next evolution after cookies. Cookies were being used to remember user's preferences and some searching data about them. When those client-side cookies reached their capacity, session storage starting taking over for that sort of data storage about a user.

# Resources
[A new session should be created during user authentication](https://rules.sonarsource.com/typescript/type/Vulnerability/RSPEC-5876)
