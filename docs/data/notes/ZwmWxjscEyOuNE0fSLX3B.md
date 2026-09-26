
#### Serving static files
ie. Caddy performing the duties of a static file server
```conf
example.com {
    # if `root` omitted, pwd (of Caddyfile) is assumed
    # `*` is a wildcard matcher, so it matches all requests.
    root * /var/www
    file_server
}
```

#### Reverse proxy load balance
This will round-robin load balance requests to 3 different hosts on the same local network
```conf
reverse_proxy /api/* 192.168.0.30:80 192.168.0.31:80 192.168.0.32:80 {
	lb_policy round_robin
}
```
