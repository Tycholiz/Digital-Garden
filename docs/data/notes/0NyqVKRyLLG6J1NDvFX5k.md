
# Session
- When user logs in, the server creates a session for the user, and stores it a database of some sort. The server then sends the sessionId to the client, and it is stored in a cookie. That cookie gets sent with each request to the server, where it is checked against what is in the db.

Sessions are stateful (unlike [[JWTs|auth.jwt]], which are stateless)
- This means that one of the communicating parties must keep track of (store) information about the current state, and save information about session history.

Sessions piggyback off of stateless HTTP requests to make a stateful connection.
- anal: the way TCP piggybacks on top of IP

When there is an active session, the server responds to the request with an ETag (entity tag), which is an id corresponding to a particular "version" of data. If the data arrives and it is identical to the ETag, then we know it is the same as the cached version.

In a session-based workflow, a session is active until the user hits the logout button

sessions are not an option for mobile apps.

## Persistent Session Store
a place to store all session-related data (but persistent so it doesn't get erased when the user closes the tab or app)
- session store is like the next evolution after cookies. Cookies were being used to remember user's preferences and some searching data about them. When those client-side cookies reached their capacity, session storage starting taking over for that sort of data storage about a user.

# Resources
[A new session should be created during user authentication](https://rules.sonarsource.com/typescript/type/Vulnerability/RSPEC-5876)
