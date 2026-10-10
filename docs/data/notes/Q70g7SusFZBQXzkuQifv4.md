
## Overview
In a nutshell Next.js is a combination of 3 components: 
- A server that renders React apps, 
- a compiler that bundles your FE and BE code 
- a CLI tool to manage all these tasks for you

create pre-rendered react websites, offered from SSR

JSX is rendered already into HTML on the server, and is sent to the client to be displayed
- vanilla-react will do everything at runtime.

Next takes care of routing for us
- we define new pages in the `pages/` directory, and next picks up on them automatically, giving us the url out of the box.
	- next defines a component `<Link />` we can use to handle routes

Next is smart and will not re-render the same content, if it has rendered it already

If we had a function in a Next.js component that referred to the `window` object, we would get errors. This is because that code is being run on the server, which of course has no concept of the browser's global variables. However, if we were to use `window` in the `useEffect` call, we would indeed get access to the `window` object. This shows that `useEffect` is still done client side, even though functions are understood server-side

### How does it work?
When a client hits a URL, the Next.js server sends back an `index.html` file, which contains the initial static render of the React components from the associated `index.jsx` file of your source code.
- the initial static render is what the page looks like, considering the initial state/props that are used to build the components.
- the received `index.html` file also contains references to multiple JavaScript files, which load up React and your root `App` component, and attach that `App` to the html root tag (this is known as [[hydration|react.patterns.hydration]]).
- From there the app behaves like any other React app.

## Resources
- [Advanced rendering patterns](https://www.youtube.com/watch?v=PN1HgvAOmi8)