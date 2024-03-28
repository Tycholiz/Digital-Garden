
TLS consists of two phases: 
1. secure connection establishment 
2. the use of that encrypted channel for further communication.

A TCP handshake and connection must be established before messages to create a TLS connection are exchanged.
- the handshake is done in one round trip. 
- after the first two steps, all messages are encrypted.

TLS involves public-key cryptography to establish a shared secret that is then used to encrypt future communication

TLS (Transport Layer Security) replaced SSL (Secure Sockets Layer), which is deprecated

spec:TLS is an agreement (protocol) between 2 IP addresses (your own and the web server you are connecting to).

Certificates are bound to domain names instead of IP addresses, so the "Not Secure" (`ERR_CERT_COMMON_NAME_INVALID`) warning will still appear if you connect via an IP address.

### SSL Termination
- the act of data reaching the end of the SSL chain and getting decrypted (or offloaded) so the recipient can read the data.
	- happens at the server end of an SSL connection
- SSL termination helps speed the decryption process and reduces the processing burden on backend servers.
![](/assets/images/2021-03-09-09-46-03.png)

### Wildcard SSl Certificate
a single ssl certificate that lets us have SSL on any `*.mydomain.com`

### UE Resources
- [self-signed certificates](https://medium.com/@jonatascastro12/understanding-self-signed-certificate-in-chain-issues-on-node-js-npm-git-and-other-applications-ad88547e7028)
- [cloudflare guide on SSL](https://developers.cloudflare.com/ssl/)

### E Resources
[setting up a private CA](https://www.digitalocean.com/community/tutorials/how-to-
