
### Claim
- the core of a JWT, since they are the data contained in the JWT
- claims are pieces of information that are "claimed" about a subject (most often a user). In other words, they are just properties of an object.
	- ex. name, sub, admin
- the claim refers to the key, not the value
- usually not encrypted, meaning if we don't use https, this information is potentially compromised.
- anyone will be able to decode them and to read them, we cannot store any sensitive data in here
	- not an issues because of the secret
- ex:
```
{
	"sub": "1234567890",
	"name": "John Doe",
	"admin": true
}
```
- when the server receives a JWT from an HTTP request's authorization header, like so:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhIjoxLCJiIjoyLCJjIjozfQ.hxhGCCCmGV9nT1slief1WgEsOsfdnlVizNrODxfh1M8
```
it will verify the token using the secret, and then will serialize the claims in that token to the database. Therefore, we will be able to access the data in `current_settings` (if using Postgres)

- claims can be used as a means to differentiate users. Imagine we are building a postgres database and decide that all signed-in users will have the group role `user_login`. We can use the claims in the jwt to distinguish users
- in postgres, a claim can be retrieved with `current_setting('request.jwt.claim.email', true)`

*Registered claim* - recommended pre-defined claims
	- ex. **iss** (issuer), **exp** (expiration time), **sub** (subject), **aud** (audience)
	
