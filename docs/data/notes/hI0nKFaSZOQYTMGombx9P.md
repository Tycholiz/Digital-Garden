
spec: In traditional server-side rendered applications (think Express-Pug, Ruby on Rails), each value in the HTML is generated on the server, and sent to the client. That means if the page changed, it's because the server sent some new HTML to the client. With Next, the initial render is done via the server at compile time, but the same React code used to generate that HTML is also included on the client (ie. the bundle.js is included in Nextjs, just like it is in vanilla React)
- This compiled client-side javascript code (originally React code) gets run on the client, building up a picture of what the world should look like. Then it is compared to the HTML in the document. This process is **Rehydration**.

### Render vs Rehydration
In a render, vanilla React detects when there are changes to state or props. When there are, React updates the DOM.
In a rehydration, React assumes the DOM won't change; It's just trying to adopt the existing DOM.

# E Resources
[Quality resource on rehydration. Uses the backdrop of a dynamic Navbar to explain](https://www.joshwcomeau.com/react/the-perils-of-rehydration/)
