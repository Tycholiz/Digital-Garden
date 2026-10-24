
### Timing Attack
Consider the following email/password auth flow:
1. user sends username and password to login.
2. Server receives username, looks up a user.
3. If a user does not exist, send generic error
4. If a user is found, hash the password that was sent, and check for a match.
5. If passwords don’t match, error generically as per 3.
6. If match, login.

There is an inherent problem with this flow. The point of sending the user a generic error is that if it's an attacker, we aren't giving them any additional knowledge about why the login was rejected. However, the fact that we check the username first (and throw an error if it doesn't exist) means that failed authentication attemps arising due to incorrect username will be faster than failed attempts arising due to correct username + incorrect password (since password hashing is inherently expensive). Attackers can use this discrepancy in response time to surmise that the account exists. The trick is to hash the password first, regardless of whether or not a user is matched.
- Newer methods like argon2 (bcrypt alternative) make this redundant