
The signature is the key part of the JWT and is the part that provides its level of security.
- The signature is what enables a fully stateless server to be sure that a given HTTP request belongs to a given user, just by looking at a JWT token present in the request itself, and without forcing the password to be sent each time with the request.
- The signature is what proves that the payload is correct, and that it was actually sent by a given third party.

While we can take the JWT header and payload and decode it with a base64 decoder, we cannot do the same with the signature. 
- in other words, it is not JSON; it is a cryptographic signature.

The JWT signature is created using the header, the claims AND the secret. Therefore, this unique combination creates a hash, and if something in the claim were to change, then the signature would be different and would no longer match up. 
- What this means is that if the 1st and 2nd set of the JWT don't change, than neither will the 3rd (of course assuming the secret remains unchanging)
- Therefore, the signature of a JWT can only be produced by someone in possession of both the payload (plus the header) and a given secret key.
- in practice, a JWT will always be different between instances of the same user signing in, since the `exp` variable will always be different

<!-- TODO: broken image -->
![8ded9f2b6239d7464fa7e85badce77c6.png](:/fa5c3e7e2d95477e85f51bed49c6b4d9)

The signature is a MAC (Message Authentication Code)

Here is how the signature is used to ensure Authentication:
1. the user submits the username and password to an Authentication server, which might be our Application server, but it's typically a separate server
2. the Authentication server validates the username and password combination and creates a JWT token with a payload containing the user technical identifier and an expiration timestamp
3. the Authentication server then takes a secret key, and uses it to sign the Header plus Payload and sends it back to the user browser
4. the browser takes the signed JWT and starts sending it with each HTTP request to our Application server
5. the signed JWT acts effectively as a temporary user credential, that replaces the permanent credential wich is the username and password combination

From there, here is what our Application server does with the JWT token:
1. our Application server checks the JWT signature and confirms that indeed someone in possession of the secret key signed this particular Payload. They know this because they can take the JWT header plus the payload and hash it together with the password (if HS256, the password must be the same as the possessed by the original issuer)
2. The Payload identifies a particular user via a technical identifier
3. Only the Authentication server is in possession of the private key, and the Authentication server only gives out tokens to users that submit the correct password
4. therefore our Application server can safely be sure that this token was indeed given to this particular user by the Authentication server, meaning that it's indeed the user as it had the right password
6. The server proceeds with processing the HTTP request assuming that it indeed it belongs to that user

### Signature types
There are many types of signature for JWTs, but two main types: HS256 and RS256.

#### HS256
HS256 is a [[cryptographic hashing function|crypt.hashing]]
- It is a symmetric algorithm, which means that there is only one private key that must be kept secret, and it is shared between the two parties
- can be brute forced if the input secret key is weak (could also be said about many other key-based technologies)
- requires the existence of a previously agreed upon secret between the original issuer of the JWT and any other server consuming JWTs (e.g. application server). Therefore, to change the secret is non-trivial.
  - this means everyone who has the secret password can create JWTs. This also means there are more places where the secret can be stolen.

#### RS256 (preferred)
RS256 uses a [[public-key/private-key|crypt.public-key]] paradigm. The implication is that while only the authentication provider can sign (create) JWTs, the servers that consume JWTs can only validate them.
- It is an asymmetric algorithm, which means that there are two keys: one public key and one private key that must be kept secret
  - The auth server has the private key used to generate the signature, and the consumer of the JWT retrieves a public key from the metadata endpoints provided by the auth server and uses it to validate the JWT signature.
- the auth server cannot validate tokens.

RS256 is preferred, since:
- you are sure that only the holder of the private key (ie. the auth server) can sign tokens, while anyone can check if the token is valid using the public key.
- if the private key is compromised, you can implement key rotation without having to re-deploy your application or API with the new secret (which you would have to do if using HS256).

RS256 signatures use RSA keys (which uses one key to encrypt and another to decrypt)
- this isn't a hashing function, since the operation is reversible.