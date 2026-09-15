
Pages and layouts can export a `load()` function that runs before the component is created.
- This function runs both during server-side rendering and in the client
- An example of its usage would be to allow us to get page data without showing a loading spinner and fetching data in `onMount`.
- `load` is similar to `getStaticProps` or `getServerSideProps` in Next.js, except that it runs on both the server and the client.

If load returns nothing, SvelteKit will fall through to other routes until something responds, or will respond with a generic 404.

SvelteKit's load receives an implementation of fetch, which has the following special properties:
- it has access to cookies on the server
- it can make requests against the app's own endpoints without issuing an HTTP call
- it makes a copy of the response when you use it, and then sends it embedded in the initial page load for hydration

code inside `load` blocks should never reference `window`, `document`, or any browser-specific objects

code inside `load` blocks should not directly reference any API keys or secrets, which will be exposed to the client, but instead call an endpoint that uses any required secrets

spec: If we want to make HTTP calls in our `load` function, we will have to import a created API endpoint (`export async function get()...`) and use it in `load`. 

The `load` function is reactive, and will re-run when its parameters change, but only if they are used in the function.
- Specifically, if page.query, page.path, session, or stuff are used in the function, they will be re-run whenever their value changes. 
- Note that destructuring parameters in the function declaration is enough to count as using them. 

If you return a `Promise` from load, SvelteKit will delay rendering until the promise resolves.

* * *

### Signature of `load()`
The `load` function receives an object containing four fields — `page`, `fetch`, `session` and `stuff`

#### page
```ts
interface page: {
    host: string;
    path: string;
    params: PageParams;
    query: URLSearchParams;
};
```
So if the example above was `src/routes/blog/[slug].svelte` and the URL was `https://example.com/blog/some-post?foo=bar&baz&bizz=a&bizz=b`, the following would be true:
- `page.host === 'example.com'`
- `page.path === '/blog/some-post'`
- `page.params.slug === 'some-post'`
- `page.query.get('foo') === 'bar'`
- `page.query.has('baz')`
- `page.query.getAll('bizz') === ['a', 'b'`]

#### stuff
`stuff` is passed from layout components to child layouts and page components and can be filled with anything else you need to make available. For the root `__layout`.svelte component, it is equal to `{}`, but if that component's load function returns an object with a `stuff` property, it will be available to subsequent load functions.

```ts
export interface LoadInput<
	PageParams extends Record<string, string> = Record<string, string>,
	Stuff extends Record<string, any> = Record<string, any>,
	Session = any
> {
	page: {
		host: string;
		path: string;
		params: PageParams;
		query: URLSearchParams;
	};
	fetch(info: RequestInfo, init?: RequestInit): Promise<Response>;
	session: Session;
	stuff: Stuff;
}
```

```ts
export interface LoadOutput<
	Props extends Record<string, any> = Record<string, any>,
	Stuff extends Record<string, any> = Record<string, any>
> {
	status?: number;
	error?: string | Error;
	redirect?: string;
	props?: Props;
	stuff?: Stuff;
	maxage?: number;
}
```
