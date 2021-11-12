
ALPN is a TLS extension that allows the application layer to negotiate which protocol should be performed over a secure connection
- A benefit of this is that it inherently avoids additional round trips

ALPN is sent on the initial TLS handshake (Client Hello), and it shows which protocols the client (eg. browser) supports