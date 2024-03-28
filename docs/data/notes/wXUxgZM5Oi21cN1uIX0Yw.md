
The JSON Web Key (JWK) is a JSON object that contains a well-known public key which can be be used to validate the signature of a signed JWT.
- The members of the object represent properties of the key, including its value.
- If the issuer of your JWT used an asymmetric key (like RS256) to sign the JWT, it will likely host a file called a JSON Web Key Set (JWKS). The JWKS is a JSON object that contains the property keys, which in turn holds an array of JWK objects.

The service may only use one JWK for validating web tokens, however the JWKS may contain multiple keys if the service rotates signing certificates.
- we can use a service like Auth0 or Okta to store our JWKs, which will be located at an endpoint hosted at their domain. 
- Any time your application validates a JWT, it will attempt to retrieve the JWK(S) from the issuer in order to ensure the JWT signature matches the content.

JWKs enable us to rotate keys.

Example of a JWK representing an RSA public key:
```json
{
  "alg": "RSA",
  "kid": "1",
  "use": "sig",
  "n": "qDABcd7-bvFz1MC1Upu5r1QhIwe8U7Gd0bY5",
  "e": "AQAB"
}
```

Where:
- `alg`: specifies the type of cryptographic key. In this example, it's "RSA".
	- sometimes `kty` is used, for Key Type
- `kid`: Key ID, a unique identifier for the key.
- `use`: How the key is intended to be used. In this example, it's "sig" for signing.
- `n`: Modulus, a large integer value that is part of the RSA public key.
- `e`: Exponent, a small integer value that is part of the RSA public key.

## JSON Web Key Set (JWKS)
A JWKS is an object holding a set of JWKs containing the [[public keys|crypt.public-key]] used to verify any JWT issued by the Authorization Server and signed using the RS256 signing algorithm.

The auth server (e.g. AWS Cognito, Auth0) exposes a JWKS endpoint for each tenant, which contains the JWK used to verify all JWTs issued by the auth server for that particular tenant.

Example JWKS:
```json
{
  "keys": [
    {
      "alg": "RSA",
      "kid": "1",
      "use": "sig",
      "n": "qDABcd7-bvFz1MC1Upu5r1QhIwe8U7Gs3hhdk93d",
      "e": "AQAB"
    },
    {
      "alg": "RSA",
      "kid": "2",
      "use": "sig",
      "n": "t5yOfCnYfJzFREvSxqOUHzUzL5KkHzzx2JgYB49",
      "e": "AQAB"
    }
  ]
}
```