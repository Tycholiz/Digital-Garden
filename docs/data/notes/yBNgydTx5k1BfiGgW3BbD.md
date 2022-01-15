
**Federated Identity** is the idea that we can link a person's identity across multiple distinct identity management systems.
- ex. we can link identity between Atlassian, Microsoft, and AWS products.

Technologies used to implement Federated Identity include:
- [[SAML|security.federated-identity.SAML]]
- OAuth
- Security Tokens (Simple Web Tokens, [[JWWTs|auth.jwt]], and SAML assertions)

Federated identity is related to single sign-on (SSO), in which a user's single [[auth token|auth.tokens]] is trusted across multiple IT systems
- SSO is a subsets of federated identity management, as it relates only to authentication and is understood on the level of technical interoperability and it would not be possible without some sort of federation.
