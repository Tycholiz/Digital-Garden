
Pages typically generate HTML to display to the user (as well as any CSS and JavaScript needed for the page).
By default, pages are rendered on both the client and server, though this behaviour is configurable.

Since pages are Svelte components, they are of filetype `.svelte`

By default, when a user first visits the application, they will be served a server-rendered version of the page in question, plus some JavaScript that 'hydrates' the page and initialises a client-side router. From that point forward, navigating to other pages is handled entirely on the client for a fast, app-like feel where the common portions in the layout do not need to be rerendered.

The filename determines the route. 

Dynamic parameters are encoded using `[brackets]`. For example, a blog post might be defined by `src/routes/blog/[slug].svelte`
- that parameter can then be accessed in 2 ways: a load function, or the page store

A good structure is to have minimal information in your page `.svelte` file, and import the entirety of the visual html from a hidden file (`_Editor.svelte`), like this:
```js
<script context="module">
	export function load({ session }) {
		if (!session.user) {
			return {
				status: 302,
				redirect: `/login`
			};
		}

		return {};
	}
</script>

<script>
	import Editor from './_Editor.svelte';
	let article = { title: '', description: '', body: '', tagList: [] };
</script>

<Editor {article}/>
```