
## Overview
The primary role of the CA is to digitally sign and publish the public key bound to a given user. This is done using the CA's own private key, so that trust in the user key relies on one's trust in the validity of the CA's key.
- When the CA is a third party separate from the user and the system, then it is called the Registration Authority (RA), which may or may not be separate from the CA.

The CA is an entity responsible for issuing digital certificates that verify identities on the internet.
- ex. there is a CA in our personal machine.

The root certificate belongs to the CA, and comes pre-downloaded in most browsers and is stored in what's called a "trust store".
- The root certificates are closely guarded by CAs.

a CA is an organization that stores public keys and their owners, and every party in a communication trusts this organization (and knows its public key)
- When we navigate to a trusted site, the browser will have already had the public key of the CA, which it can then use to verify the signature, implying that the certificate and its contained public key are trustworthy

If you communicate with HTTPS, FTPS or other TLS-using servers using certificates in the CA store, you can be sure that the remote server really is the one it claims to be.

The CA certificate file is usually called `ca.pem` or `cacerts.pem`

### Purpose
CA's are used to sign certificates that enable HHTPS
- In essence, the CA is responsible for saying "yes, this person is who they say they are, and we, the CA, certify that"
- CA servers can manage certificate enrollment requests from customers, and are able to issue and revoke digital certificates
- CA servers support various certificate templates, such as SSL (both server and client), email signing/encryption, etc
- to authenticate the recipient of the certificate, HTTPS servers typically use a technique called "domain validation"
	- This is where the domain name of the applicant is validated by proving that it owns and has control over a DNS domain. After this validation, a Domain Validated Certificate (DV) is issued.
- A CA issues digital certificates that contain a public key and the identity of the owner
	- the certificate being issued is confirmation (by the CA) that the public key within the certificate belongs to the entity noted in the certificate.
	- spec: the CA has the private key
- the CA market is very fragmented, and government-related activities (govt forms that verify identity) will have their own CA. For commercial use, the CA market is dominated by a few players (Let's Encrypt, GoDaddy, VeriSign are two of them)
- ex. when using DigitalOcean managed Postgres, we are given a `ca-certificate.crt` from DigitalOcean. This is the certificate we must pass from our application server. The reason this works, is because the certificate is signed by one of the CAs that is trusted by the DigitalOcean server.

### Chain of Trust
- example chain:
```
GlobalSign → Google CA → neverforgetapp.co
```

- The idea of Chain of Trust is that the Root CA can "trust" other Intermediate CAs (ICA) to issue certificates (which are signed by the Root CA). Though the ICAs themselves are not trustworthy, they are trusted by a trustworthy entity (the Root CA), so the certificates they issue are also considered trustworthy.
- The Root CA is kept behind many layers of security
	- if private keys are compromised, then all certificates based on the Root CA are compromised as well. For this reason, we use an Intermediate CA
- In the browser, if we click on the lock to the left of the URL, we can see the certificate chain
	- The one at the bottom (leaf node) of the tree is the SSL certificate (`*.neverforgetapp.co`), which was issued by the ICA directly above it (`Google CA`)
		- The SSL certificate is signed with the private key of the ICA (`Google CA`). Once this is done, it is sent back to `neverforgetapp.co`.
		- All certificates contain the public key of the entity that the certificate is for, and the private key of the entity that signed the certificate.
			- ex. the SSL certificate (the end of the chain) contains the public key of `neverforgetapp.co`, and is signed by Google CA; Google's CA certificate contains Google's public key and is signed by GlobalSign.
	- In the browser's Certificate Manager, we can see that GTS CA 101 is not listed as a trusted CA. Therefore, this Intermediate CA needs to be verified by a Root CA, which it is (GlobalSign). In the Certificate Manager, we can see that this is indeed trusted. This is the chain of trust.
- In our example chain, there are 2 certificates issued: the Google ICA certificate, and the SSL certificate (to `neverforgetapp.co`). These both get sent to `neverforgetapp.co` and are installed on the webserver.
	- When someone visits `neverforgetapp.co` with HTTPS, both certificates are sent back to the browser. It starts by verifying the certificate that is higher up in the chain (closer to the root), which is the Google ICA certificate. It sees that GlobalSign has signed this certificate, whom it recognizes as a trusted Root CA.
		- The browser has the public keys (and the certificates, of which they are a part) of all Root CAs, so it can unencrypt the Google ICA certificate. This proves that this ICA is trustworthy.
		- the browser trusts all Root CAs in its list. The fact that a Root CA in that list has signed the Google ICA certificate means that the browser automatically trusts that ICA.
			- As a consequence of unecrypting the certificate, the browser gets the public key from Google ICA.
		- Now that Google ICA has been considered trustworthy, the browser then looks to the `neverforgetapp.co` SSL certificate. It uses the public key issued by Google ICA to decrypt the certificate. It sees that Google ICA has signed this certificate, making it trustworthy.
		- Now, the browser gets the public key from `neverforgetapp.co`, generates a symmetric key, encrypts it (using the public key), and sends it to the `neverforgetapp.co` webserver.
			- since the `neverforgetapp.co` webserver has the private key, it is able to decrypt it, which yields the same symmetric key. Now, both the browser and the `neverforgetapp.co` webserver have the same symmetric key that is used to exchange information securely.

### Private CA
- There are 2 versions, public and private CA
	- public are for verifying the identity of websites and other services that are provided to the general public
	- private are for closed groups and private services.
- spec:While in production we will use a public CA, we may want to configure a private CA in development and staging environments so those environments match the production environment, and also have HTTPS connections.
- we need to build a private CA when our programs require encrypted connections between client and server.
	- with the private CA, we can issue certificates to users, servers, or individual programs and services within your infrastructure.
		- ex. OpenVPN would use its own private CA
		- ex. we can configure our web server to use certificates issued by a private CA to make development and staging environments match production servers that use TLS to encrypt connections.

### Bank Example
When a user logs into an HTTPS enabled bank, they receive a public key. The public key is used to create a temporary shared symmetric encryption key, which is used to encrypt all messages sent from the client to the server. These messages are enciphered with the bank's public key in such a way that only the bank has the private key required to decrypt them. For the rest of the session, the (disposable) symmetric key is used to encrypt messages from client to server.

## E Resources
- https://www.howtouselinux.com/post/exploring-unable-to-get-local-issuer-certificate
