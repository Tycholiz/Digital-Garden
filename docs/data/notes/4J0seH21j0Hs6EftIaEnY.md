

There are two basic concepts to Sveltekit:
- Each page of your app is a Svelte component
- You create pages by adding files to the `src/routes` directory of your project. These will be server-rendered so that a user's first visit to your app is as fast as possible, then a client-side app takes over

## Router
At the heart of SvelteKit is a filesystem-based router. 

There are two types of route — pages and endpoints.

* * *

## Misc
The value of Sveltekit is similar to the value of Next, but even extending beyond that. Sveltekit takes care of the modern best-practices surrounding web development and does it for you, including build optimizations, so that you load only the minimal required code; offline support; prefetching pages before the user initiates navigation; and configurable rendering that allows you to generate HTML on the server or in the browser at runtime or at build-time.

A filename that has a segment with a leading underscore, such as src/routes/foo/_Private.svelte or src/routes/bar/_utils/cool-util.js, is hidden from the router, but can be imported by files that are not.

# Resources
[Realworld Sveltekit app (Medium clone)](https://github.com/sveltejs/realworld)