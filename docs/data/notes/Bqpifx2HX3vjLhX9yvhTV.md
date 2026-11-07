
## Rendering
To "render a component" is to run the function that defines the component.
- The JSX you return from that function is like a snapshot of the UI in time. Its props, event handlers, and local variables were all calculated using its state at the time of the render.
- therefore, since state persists between renders, state actually lives in React itself— not within the function (ie. the component).
    - `useState` is still called on each render. This helps to illustrate the fact that `useState` is not creating the state, but is instead accessing it. When you call `useState`, React gives you a snapshot of the state for that render.

When React re-renders a component:
- React defines variables and calls your function again (unless cached with [[react.lang.hooks.useMemo]] or [[react.lang.hooks.useCallback]], respectively)
- Your function returns a new JSX snapshot.
- React then updates the screen to match the snapshot your function returned.

A client-side rendered React app starts out as a simple html file with virtually nothing but `<script>` tags in its `<body>`. When the browser downloads the base HTML, it runs the scripts (ie. the bundle/chunks), and builds up the rest of the HTML content (ie. the `<body>`, and everything between).
- Once the browser downloads and parses those scripts, React will build up a picture of what the page should look like, and inject a bunch of DOM nodes to make it so.
- therefore, a browser must be Javascript-enabled for client-side rendering to work. If we are using [[general.patterns.SSR]], like [[nextjs]] does, we can load the pages (but not interact with them) without being Javascript-enabled.

### Virtual DOM
- note: the virtual DOM in React in distinct from the [[browser.DOM.shadow]], which is a browser technology for scoping variables and CSS.

The virtual DOM is an object that keeps an ideal version of the UI in memory. It is then synced with the real [[browser.DOM]] by a library, such as ReactDOM.
- this process of synchronization is called *reconciliation* (the reconciliation engine of React 16+ is called *Fiber*)
- this idea of a virtual DOM is what makes React declarative. We no longer have to imperatively manipulate an attribute of a DOM node, or handle events. Instead we use a virtual DOM to update values that represent these actual DOM nodes, and when the process of reconciliation happens (ie. on re-renders), the DOM is updated for us.

Virtual DOM is more of a pattern, than a specific technology.
- usually, Virtual DOM refers to React Elements, since they are the objects representing the user interface.
    - recall: a React Element is simply an object that describes a UI element. An HTML node can be generated from a React Element.
- internally, React also uses objects called *fibers* that are used to hold additional information about the component tree.

When we talk about "re-rendering" in react, we are talking about

React splits all work into the "render phase" and the "commit phase".
- Render phase is when React calls your components and performs reconciliation.
- Commit phase is when React touches the host tree. It is always synchronous.

* * *

In React, anything other than updating the page is considered a side-effect. If you’re not using React to update state or render HTML, that’s a side effect. It’s any non-React thing.

Images that you are going to import inside of components should be in your `src/` directory, not `public/`

# UE Resources
- [Implementing Pub/Sub in React](https://www.pluralsight.com/guides/how-to-communicate-between-independent-components-in-reactjs)
    - [[pubsub Dendron node|general.patterns.messaging.pubsub]]
- [React Fiber (reconciliation engine) Architecture](https://github.com/acdlite/react-fiber-architecture)
