
We should probably have the "disable cache" checkbox enabled. This will give us a more realistic view of the actual user experience.

When looking at the load amounts, there is two numbers. 
- *DOMContentLoaded* - how much data is in the browser
    - the time it takes for the initial HTML document to be completely loaded and parsed, and for the browser to have constructed the DOM for the page.
    - doesn't including time taken for external resources like images to finish loading.
- *Load* - considers when all resources on the page, including images, stylesheets, scripts, and other assets, have been fully loaded and rendered by the browser.
    - this is more significant in terms of debugging performance
    - This is typically used to determine when it's safe to interact with or manipulate the fully loaded DOM. 

## Unofficial APIs
Often companies don't offer open APIs. We can "open it up" to ourselves by mimicking the request as if it's coming from us interacting with the webpage.
1. In the website's UI, do the interaction that will trigger a network request (eg. clicking a button)
2. In the network tab, find the request and gather the API endpoint that was hit.
3. Right-click and *copy as* fetch/cURL. We now have a fully formatted request that has all of the headers needed to make a successful request.

## Image caching status
We can check to see if image caching is working or not (eg. if we are using Cloudflare image caching worker)
1. In Network tab, filter for images, and find the image in question
2. Check response headers for an indicator (in this example, `cf-cache-status=hit`) that the image came from a cached location.

* * *

When right-clicking a request, we get the option to `Copy As...`. If we `copy as Node.js fetch`, we also get anything that came along with the request (inc. all of the cookies, session Ids, headers etc).
- a normal fetch request doesn't include these, because it assumes we are firing off the request from the browser. However, if we are making a fetch request from a server, then we would manually need to send those cookies/headers etc along.
