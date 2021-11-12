
# Let's Encrypt
## Plugins
Certbot supports 2 types of plugins for obtaining and installing certificates: authenticators and installers
- some plugins can do both, such as the Apache and Nginx plugins

### Authenticator
- Authenticators are plugins used with the `certonly` command to obtain a certificate, validating that we own the domain we are requesting a certificate for. It then obtains the certificate for that domain, and places the certificate in the `/etc/letsencrypt` directory on your machine
	- The authenticator does not install the certificate (it does not edit any of your server’s configuration files to serve the obtained certificate)
- If we list multiple domains to authenticate, they will all be included in a single certificate by default.

### Installer
- Installers are Plugins used with the `install` command to install a certificate.
- These plugins modify the webserver's configuration in order to server the site over HTTPS 

## Certificates
- All generated keys and certificates can be found on the host that serves the application. 
	- found in `/etc/letsencrypt/live/$domain` if using Let's Encrypt
- note: `pem` is a type of encoding

### privkey.pem
This is the private key for the certificate 
- This is what Apache needs for `SSLCertificateKeyFile`, and Nginx for `ssl_certificate_key`

### fullchain.pem
This is the full list of certificates, including the server certificate (a.k.a Leaf Certificate or End-Entity Certificate)
- the server certificate is the first one listed. It is followed by intermediary certificates. 
- This is what Apache needs for `SSLCertificateFile`, and what Nginx needs for `ssl_certificate`.

## Concepts
### ACME
- ACME is a communications protocol for automating interactions between CAs and their users' webservers.
	- This allows automated deployment of public key infrastructure.
- Certbot is an example of an ACME client

### Challenge
- Challenges are a way for the Let's Encrypt servers to validate that you own the domain.
- ex. HTTP-01 Challenge, DNS-01 Challenge
- We only need one.

HTTP-01
- The webserver proves it controls the domain by provisioning resources on its filesystem. The ACME server then challenges the webserver to provision a file at a specific path. If the webserver is able to do that, it is proof that the domain is under the webserver's control.
- When our webserver gets a token from Let's Encrypt, the webserver creates a file at `http://<YOUR_DOMAIN>/.well-known/acme-challenge/<TOKEN>`
	- this file also includes a thumbprint of your account key
- Once our webserver tells Let’s Encrypt that the file is ready, Let’s Encrypt tries retrieving it. If successful in doing so, then we are able to issue the certificate.
- This is the most common type of challenge.
