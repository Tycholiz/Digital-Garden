
## Matchers
A request matcher is used to filter requests.

Syntax-wise, matchers immediately follow directives

## Addresses
The address part may be any of these forms:
- `host`
- `host:port`
- `:port`
- `/path/to/unix/socket`
    - ex. `unix//path/to/socket`

some config fields may allow us to specify a port range, like `:8080-8085`

## Directives

### root
sets the root path of the site
- the root path (ie. root directory) holds all the files related to serving an application, whether they are private or public, (with respect to being exposed to the internet).

specifying multiple `root`s in the same block is legal. 
- Each `root` directive is mutually exclusive with others in the same block.
    - multiple `root`s will not cascade and overwrite each other.

`root` is usually used together with `file_server`, since it does not enable serving static files on its own.
```
root * /var/www
```

### file_server
`file_server` works by taking the request's URI path and appending it onto the root path

ex. if a user visits `https://example.com/jokes`
With the Caddyfile:
```Caddyfile
root * /var/www
```
Caddy will serve content from `/var/www/jokes`