
Suspense refers to React’s ability to “suspend” rendering while components are waiting for something, and display a loading indicator

Imagine a component that needs to do some asynchronous task before it can render, e.g. fetching some data.
Before Suspense, such a component would keep an isLoading state and return some kind fallback (an empty state, skeleton, spinner, ...) based on it.
With Suspense, a component can now, during rendering, shout "HALT! I'm not ready yet. Don't continue rendering me, please - here's a pager, I'll ping you when I'm ready!1"
React will walk up the tree to find the nearest Suspense boundary, which is set by You and contains the fallback that'll now be rendered until the pager is ringing.

In the traditional way of handling data fetching, the paradigm is: "wait for Promise to resolve and run some user code". With suspense, it's "React will retry rendering when it resolves". 
- In some ways it's simpler but it takes some "unlearning" because the user loses some control they might be used to from traditional Promise-base programming.

solves the problem of having some of the data, but not being able to render until all data is in. With suspense, we can load some of the UI without all of the data having to be available.
- Suspense lets you pause rendering a component while it’s loading async data

React Suspense will specify a fallback component in case the main component suspends. 

![28a01e0b811e84e2c3c20a4faca82641.png](:/3b101f50bf264119a5a3d57e387812f4)

## Process for data fetching and rendering in browser
1. Browser downloads the code
2. React begins to render the `<Home />` component
3. React will attempt to use the data needed for the component, then will realize that the data doesn't exist yet. React will then look up the component tree, find its nearest Suspense boundary. Then it will then render the fallback component, and fetch the data simultaneously.
4. Once data has been fetched (signified by a function like `useQuery` having been completed), React will render `<Home />` (which contains an `<img />` tag)
5. The browser sees the `<img />` tag and makes a fetch to get that image.
* * *
- If we have multiple components within a `<Suspense />`, then we have to wait
    for all components inside to be ready before anything renders. Conversely if
    each component is nested within its own `<Suspense />`, then each can render
    when it is done
```jsx
<Suspense fallback={<DotLoader />}>
    <NewsFeed />
</Suspense>
```

- If we want to control the order in which something will render, we can use a
    `<SuspenseList>` component. This component takes a prop `revealOrder`.
    - if we set `revealOrder="forwards"`, then we ensure that the first
        component will always render first. 
    - We might want to do this if component1 and component2 are stacked on top
        of each other, and component1 has dynamic dimensions (content determines
        the height). If component2 were to load first, then once component1
        loads, it would push everything down.
        - ex. Facebook's `<Composer>` and `<NewsFeed>`
- "In the long term, we intend Suspense to become the primary way to read asynchronous data from components — no matter where that data is coming from."
    - from React team
  
