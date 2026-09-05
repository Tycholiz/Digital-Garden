
### Workers in the browser
#### Web workers
A web worker is a JavaScript file that runs independently of the website off of the main thread of the app

#### Service workers
A service worker is a type of web worker which acts as a proxy between the browser and the server.  It also acts as a proxy between the browser and the cache.
- Put another way, service workers act as a caching agent, and can store content for offline use.
- They also give you more control over network requests and allow you to handle push-messaging, too. Since service workers are web workers, they run independently of your app, meaning that they can run even when the app is not open. Progressive web apps use service workers, and can thus work offline or on very slow networks.

Service workers are based on JavaScript promises They also use JavaScript’s fetch and cache APIs.


