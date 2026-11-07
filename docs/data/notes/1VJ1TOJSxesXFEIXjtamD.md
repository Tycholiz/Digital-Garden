
the core of a JWT, since they are the data contained in the JWT
- claims are pieces of information that are "claimed" about a subject (most often a user). In other words, they are just properties of an object.
	- ex. name, sub, admin
	- ex. "the holder of this token is able to create/read/update/delete a specified resource"
- the claim refers to the key, not the value
- usually not encrypted, meaning if we don't use https, this information is potentially compromised.
- anyone will be able to decode them and to read them, we cannot store any sensitive data in here
	- not an issue because of the secret
```json
{
	"sub": "1234567890",
	"name": "John Doe",
	"admin": true
}
```
- when the server receives a JWT from an HTTP request's authorization header, like so:

> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhIjoxLCJiIjoyLCJjIjozfQ.hxhGCCCmGV9nT1slief1WgEsOsfdnlVizNrODxfh1M8

it will verify the token using the secret, and then will serialize the claims in that token to the database. 
- If using Postgres, this would enable access to the data in `current_settings`, e.g. `current_setting('request.jwt.claim.email', true)`

Claims can be used as a means to differentiate users. Imagine we are building a postgres database and decide that all signed-in users will have the group role `user_login`. We can use the claims in the jwt to distinguish users

*Registered claim* - recommended pre-defined claims
- **iss** (issuer)
- **exp** (expiration time)
- **sub** (subject)
- **aud** (audience)
	
## Properties
- `iss` means the issuing entity, in this case, our authentication server
- `iat` is the timestamp of creation of the JWT (in seconds since Epoch)
- `sub` contains the technical identifier of the user
- `exp` contains the token expiration timestamp