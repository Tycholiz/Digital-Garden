
Events are the native way to deal with user input in browser based web applications. Events are not part of the language of JavaScript itself, but they are part of the browser environment that JavaScript runs in

Browsers are single threaded and this single thread (The UI thread) is shared between the rendering engine and the js engine.
- Therefore, if the thing you want to do takes a lot of time, it could halt the rendering (flow and paint).
- In browsers there also exists "The bucket" where all events are first put in wait for the UI thread to be done with whatever it´s doing. As soon as the thread is done it looks in the bucket and picks the task first in line.
    - Using `setTimeout` you create a new task in the bucket after the delay and let the thread deal with it as soon as it´s available for more work.

### Ajax
Ajax is when a client-side JavaScript application running inside a web browser uses XMLHttpRequest to become an HTTP client, enabling it to make requests.
- the request is probably for JSON, rather than HTML, as is the traditional response data format for HTTP requests from a browser.

## Resources
- [What happens when typing URL into address bar](https://systemdesign.one/what-happens-when-you-type-url-into-your-browser/)