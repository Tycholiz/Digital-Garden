
OpenID Connect (OIDC) is an identity layer built on top of the OAuth2 framework
- It allows third-party applications to verify the identity of the end-user and to obtain basic user profile information. OIDC uses JSON web tokens (JWTs), which you can obtain using flows conforming to the OAuth 2.0 specifications.

While OAuth 2.0 is about resource access and sharing, OIDC is about user authentication. Its purpose is to give you one login for multiple sites.
- Each time you need to log in to a website using OIDC, you are redirected to your OpenID site where you log in, and then taken back to the website.