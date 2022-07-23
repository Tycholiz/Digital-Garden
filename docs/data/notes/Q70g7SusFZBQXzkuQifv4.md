
create pre-rendered react websites, offered from SSR

jsx is rendered already into html on the server, and is sent to the client to be displayed
- vanilla-react will do everything at runtime.

helps with SEO

next takes care of routing for us
- we define new pages in the `pages/` directory, and next picks up on them automatically, giving us the url out of the box.
	- next defines a component `<Link />` we can use to handle routes

next is smart and will not re-render the same content, if it has rendered it already

If we had a function in a Next.js component that referred to the `window` object, we would get errors. This is because that code is being run on the server, which of course has no concept of the browser's global variables. However, if we were to use `window` in the `useEffect` call, we would indeed get access to the `window` object. This shows that `useEffect` is still done client side, even though functions are understood server-side
