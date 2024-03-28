
Web servers like Apache, Nginx and Caddy can perform various enhancements to our requests
- Reverse proxy with caching
- IPv6
- Load balancing
- FastCGI support with caching
- WebSockets
- Handling of static files, index files, and auto-indexing
- TLS/SSL with SNI

Web servers are built to handle a lot of incoming requests simultaneously. Nginx and Apache each take a different approach to how they take a request from the incoming request queue and hand it off to a worker process.
