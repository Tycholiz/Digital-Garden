
## Server Certificate
Also known as SSL/TLS certificates

Used to authenticate the identity of a server to the client that is trying to connect to it.

When a server certificate gets installed on a website, HTTPS gets enabled, and the certificate chain gets created, the result of which vouches for the authenticity of the website.
- When we hop on to our computers and type in a website URL, the server certificate ensures that the data flow between our client browser and the domain we’re trying to reach stays secure

Based on PKI

## Client Certificate
Used to validate the identity of a client.
- In this way, it serves as a password, as in theory, no one should be able to produce that certificate but the true client.
- Client certs don't encrypt any data.

Client certificates exist because passwords aren't that secure.

Client certificates use the Public Key Infrastructure (PKI) for authentication (just as server certificates do).
- Difference is that client certificates don't encrypt any data; they are just for validation purposes.

Contain "Issued to" and "Issued by" fields

Based on PKI
