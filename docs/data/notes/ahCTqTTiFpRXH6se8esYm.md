
CORS is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources.
- It does this by modifying the response headers returned from the server.
- ex. if your app uses a headless CMS, then that server must be configured to allow requests from your website `www.mypersonalwebsite.com`.

By default, if you are trying to access a resource that has a different domain from the one that serves your main website, then CORS will prevent that from happening.
- To get around this, we give the server a list of allowed domain names that it will allow requests from.

When a website can access a resource or execute commands on another domain via HTTP requests, the process is called cross-origin resource sharing.
- ex. you are on a blogging website, and clicking on one of their buttons sends an HTTP request to your bank's website requesting that all money be transferred out.
- Because of CORS, this of course cannot occur. So if we want to allow certain domains to send requests, we need to put them on some sort of whitelist.

CORS is a mechanism to use HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin
- a cross-origin HTTP request is executed when it requests a resource that has a different origin (domain, protocol, or port) from its own
- ex. the front-end JavaScript code served from `https://domain-a.com` uses XMLHttpRequest to make a request for `https://domain-b.com/data.json`

In a browser console, the following code snippet will always result in a CORS error unless:
- I'm already on google.com
- Google's server has enabled cross-origin requests from whatever website (hostname) I'm on 
```js
fetch("https://google.com", function(error, meta, body){
    console.log(body.toString());
});
```

#### Enabling CORS for a host

The server is the one that enables the cross-origin requests, not the client
- [Adding CORS support to server](https://enable-cors.org/server.html)

```js
router.use(function (req,res,next) {
  console.log('/' + req.method);
  res.header("Access-Control-Allow-Origin", "https://google.com");
  next();
});
```

If you want to make a request from one domain to another, then you need to enable CORS on the server


* * *

### Terminology
Origin - the User Agent that initiated the request.

* * *

## UE Resources
[MDN guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
