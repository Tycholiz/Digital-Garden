
The idea for SSR is that there is a large chunk of our website (if not all) that is static. It is expensive to ship a bundle of javascript to the client, and expect the client to do all the work of parsing it and converting it to html. The question becomes: "what if we could just pre-render this react into html beforehand on the server, and give that content to the client when it is requested? That way, the content can be served as fast as if we were using plain HTML/CSS/Javascript.

The most common use case for server-side rendering is to handle the initial render when a user (or search engine crawler) first requests our app. When the server receives the request, it renders the required component(s) into an HTML string, and then sends it as a response to the client. From that point on, the client takes over rendering duties.

## CSR vs SSR
### CSR Approach
Using CSR, the server will send a mostly empty HTML file to begin with. Through links, it will build up the page through a series of further HTTP requests.

1. Browser requests `example.com/index.html`
2. Server responds with `<script/>` tag at end of `<body/>`
3. Browser makes an API request for the content in those script tags
4. Server responds with that content.
5. Browser makes async requests for Javascript bundles.
    - note: these bundles can be cached to speed things up in the future.

Cons of CSR:
- Loading content may take a while because requests have to travel all the way to the server, which can be very far away.
- SEO takes a hit because all search engines see is an empty HTML file.

### SSR Approach
In SSR, the whole web page is compiled on the server. The HTML is completely populated with the content before it is sent to the client.

1. Browser requests `example.com/index.html`
2. Server makes co-located API requests to populate the HTML "template" with the data it needs
3. Server sends that populated HTML back to the client.

Cons of SSR:
- A page will have to be rendered on the server and reloaded every time a new page on the site is visited, which will lead to full page reloads.
- The server will receive frequent requests, which can easily lead to the server getting flooded with requests and slowing down.

## Implementations

### with React
With simple client-side rendering, we have a root node `<div id="root">` to which the entire React app gets attached to. This node is by default empty.
With SSR, this node has some content by default. But, instead of replacing that content, we want React to attach a running application to it.

This is where hydration comes in. During hydration, React works quickly in a virtual DOM to match up the existing content with what the application renders, saving time from manipulating the DOM unnecessarily. It is this hydration that makes SSR worthwhile.

There are two big rules to hydrating an application in React.
1. The initial render cycle of the application must result in the same markup, whether run on the client or the server.
2. We must call ReactDOM.hydrate instead of ReactDOM.render in order to instruct React to hydrate from our SSR result.

### with Redux
- When using Redux with server rendering, we must also send the state of our app along in our response, so the client can use it as the initial state. This is important because, if we preload any data before generating the HTML, we want the client to also have access to this data. Otherwise, the markup generated on the client won't match the server markup, and the client would have to load the data again.
- To send the data down to the client, we need to:

1. create a fresh, new Redux store instance on every request;
2. optionally dispatch some actions;
3. pull the state out of store;
4. and then pass the state along to the client.
Redux's only job on the server side is to provide the initial state of our app.

On the server, you will want each request to have its own store, so that different users get different preloaded data.

### with Gatsby
Sometimes Gatsby is referred to as doing SSR. More realistic is that its doing SPG (Static site generation)

# UE Resource
- [Good breakdown of SSR](https://www.freecodecamp.org/news/react-server-components/)
