
## What is it?
CORS is an HTTP-header based mechanism that allows a server to indicate any origins other than its own from which a browser should permit loading resources.
- An **origin** is a combination of scheme, domain and port (ie. `https://` + `example.com` + `:443`)
  - therefore `https://example.com` and `https://api.example.com` are different origins
- It does this by modifying the response headers returned from the server.
- ex. if your app uses a headless CMS, then that headless CMS server must be configured to allow requests from your website `www.mypersonalwebsite.com`.
- note: a scheme can be a *local scheme* (`about://`, `blob://`, `data://`, `file://`) or an *http scheme* (`http://`)

When a website can access a resource or execute commands on another domain via HTTP requests, the process is called *cross-origin resource sharing*.
- ex. you are on a blogging website, and clicking on one of their buttons sends an HTTP request to your bank's website requesting that all money be transferred out.
- Because of CORS, this of course cannot occur. So if we want to allow certain domains to send requests to be handled by the server, the server needs to have some sort of whitelist of domains.

By default, if you are trying to access a resource that has a different domain from the one that serves your main website, then CORS will prevent that from happening.

In a browser console, the following code snippet will always result in a CORS error unless:
- I'm already on google.com
- Google's server has enabled cross-origin requests from whatever website (hostname) I'm on 
```js
fetch("https://google.com", function(error, meta, body){
    console.log(body.toString());
});
```

Consider that CORS restrictions don't come into play for all cross-origin resource sharing, meaning we can request resources without their consent.
- ex. `<img>`, `<script>`, `<link>`, `<iframe>`, `<video>`, `<audio>`

#### Enabling CORS for a host

The server is the one that enables the cross-origin requests, not the client
- [Adding CORS support to server](https://enable-cors.org/server.html)

- If a resource never contains private data, then it's safe to put `Access-Control-Allow-Origin: *`.
- If a resources sometimes contains private data depending on cookies, it's safe to add `Access-Control-Allow-Origin: *` as long as you also include a `Vary: Cookie` header.

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
Origin - the User Agent that initiated the request. In other words, the client.

* * *

## UE Resources
[MDN guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
