
Apache is an HTTP server (just like NGINX)

# How it works
- Apache listens to the IP addresses identified in its config file (HTTPd.conf). Whenever it receives a request, it analyzes the headers, and takes action; considering the rules specified for it in the Config file,

## Four main directories on apache server
- `htdocs` contains the files to be served to the client upon receiving HTTP requests
	- files and sub-directories under htdocs are available to the public 
- `conf` contains all server configuration files 
	- similar to Dockerfile, it is a manifesto of instructions for the apache server
- `logs` contains server logs
- `cgi-bin` contains CGI scripts

# E Resources
[primer on apache](https://code.tutsplus.com/tutorials/an-introduction-to-apache--net-25786)
