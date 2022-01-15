
### JWK (JSON Web Key)
- The JWK is a JSON object that contains a well-known public key which can be be used to validate the signature of a signed JWT
- If the issuer of your JWT used an asymmetric key to sign the JWT, it will likely host a file called a JSON Web Key Set (JWKS). The JWKS is a JSON object that contains the property keys, which in turn holds an array of JWK objects.
- The service may only use one JWK for validating web tokens, however the JWKS may contain multiple keys if the service rotates signing certificates.
	- we can use a service like Auth0 or Okta to store our JWKs, which will be located at an endpoint hosted at their domain. 
	- Any time your application validates a JWT, it will attempt to retrieve the JWK(S) from the issuer in order to ensure the JWT signature matches the content.

Login
1. user enters email/pw combination
2. email/pw sent to server. The server takes in that password, appends the salt to it, then hashes it and compares it to the hashed pw stored in the database.
3. if the passwords match, then the server will send back a signed JWT 
	- the jwt is composed of 3 parts:
		- header - usually contains type of token and signing algo used:
			```
			{
			  "alg": "HS256",
			  "typ": "JWT"
			}
			```
		- claims (aka payload)
			- this is the data that we can encode into the token
		- signature

HTTP requests
1. server receives the request, along with the JWT.
2. JWT header and claims are combined with the secret (stored on the server) to generate a "test signature", to compare against the original signature (also received in the JWT) 
