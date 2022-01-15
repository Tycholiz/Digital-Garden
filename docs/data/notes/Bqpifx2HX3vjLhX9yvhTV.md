
## Rendering
A client-side rendered React app starts out as a simple html file with virtually nothing but `<script>` tags in its `<body>`. When the browser downloads the html, it runs the scripts, which is the bundle/chunks.
- Once the browser downloads and parses those scripts, React will build up a picture of what the page should look like, and inject a bunch of DOM nodes to make it so.

When we talk about "re-rendering" in react, we are talking about the shadow DOM, not the regular DOM. Re-rendering the shadow DOM is highly performant, which is why re-rendering components shouldn't be that big of a concern.

- React splits all work into the “render phase” and the “commit phase”.
	- Render phase is when React calls your components and performs reconciliation.
	- Commit phase is when React touches the host tree. It is always synchronous.

* * *

- In React, anything other than updating the page is considered a side-effect. If you’re not using React to update state or render HTML, that’s a side effect. It’s any non-React thing.

# UE Resources
[Implementing Pub/Sub in React](https://www.pluralsight.com/guides/how-to-communicate-between-independent-components-in-reactjs)
- [[pubsub Dendron node|general.patterns.messaging.pubsub]]
