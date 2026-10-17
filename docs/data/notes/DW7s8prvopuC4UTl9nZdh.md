
Endpoints run only on the server
- This means it's the place to do things like access databases or APIs that require private credentials or return data that lives on a machine in your production network. Pages can request data from endpoints. Endpoints return JSON by default

Endpoints are written in `.js` (or `.ts`) files.

Endpoints export functions that correspond to HTTP methods.
- `get`, `post`, `del`, `put`, `patch`

> We don't interact with the req/res objects you might be familiar with from Node's http module or frameworks like Express, because they're only available on certain platforms. Instead, SvelteKit translates the returned object into whatever's required by the platform you're deploying your app to.

headers (including cookies) can be set in the return value of the HTTP method.

if we had a `routes/blog/post1.svelte` page, we could request data from `routes/blog/post1.json`
- both could be represented in the file structure as `blog/[slug].svelte` and `blog/[slug].json`, respectively.
