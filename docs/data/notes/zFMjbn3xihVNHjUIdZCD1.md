
# Common Gateway Interface (CGI)
- a specification for web servers (like Nginx, Apache) to execute command line programs that run on a server and generate web pages dynamically.
	- Such programs are known as CGI scripts or simply as CGIs
	- Normally, a CGI script executes at the time a request is made and generates HTML.
- ex. an HTTP GET or POST request from the client may send HTML form data to the CGI program (via STDIN). Simultaneously, the CGI program will receive other data like URL path and HTTP headers as a list of environment variables. 

Purpose
- The HTTP server (ex. Express) will have a directory that is designated as a document collection (ie. each file in the directory is a document).
	- These files can be sent to users accessing the website. 
- ex. if our website was www.example.com and we had a document collection stored at `/usr/local/apache/htdocs` of the local filesystem, the Web server will respond to a request for `http://example.com/index.html` by sending to the browser the (pre-written) file `/usr/local/apache/htdocs/index.html`

## FastCGI
- this is an alternative approach and variation of CGI.
- it is a protocol for interfacing interactive programs with a web server
- purpose is to reduce overhead by allowing a single, long-running process to handle more than one user request
	- result is that the server can handle more web page requests per unit of time.
- FastCGI applications remain independent of the web server.
- FastCGI can be implemented in any language that supports network sockets
	- ex. Node, PHP, Python
- both Nginx and Apache implement FastCGI
