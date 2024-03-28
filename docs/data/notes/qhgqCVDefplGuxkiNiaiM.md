
### `getInitialProps`
This function is called and returns props to the react component before rendering the templates in `/pages`. This is a perfect place for fetching the data you want for a page.

`getInitialProps` works only in files in the pages folder and are used for routing, i.e it will not be called for react components that are included in these pages

Adding a custom getInitialProps in your `_app.js` will disable Automatic Static Optimization in pages without Static Generation.

### `getServerSideProps`
When we specify this function, the Nextjs server will pre-render the html **on each request** before returning it to the client
- Inside `getServerSideProps`, you can fetch data, perform server-side operations, or do anything you need to prepare the page's data.

`getServerSideProps` returns JSON which will be used to render the page.

Use `getServerSideProps` only if you need to render a page whose data must be fetched at request time.
- If you do not need to render the data during the request, then you should consider fetching data on the client side or `getStaticProps`.

1. When a user makes a request to a page with `getServerSideProps`, the Next.js server executes this function on the server.
2. The data you fetch inside the function is then used to pre-render the page's HTML on the server, including the initial data.
3. The fully rendered HTML page is sent as a response to the client's browser.
    - When the client's browser receives the HTML, it can display the page immediately without needing to wait for client-side JavaScript to load and render the content.