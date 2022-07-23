
### `getInitialProps`
This function is called and returns props to the react component before rendering the templates in `/pages`. This is a perfect place for fetching the data you want for a page.

`getInitialProps` works only in files in the pages folder and are used for routing, i.e it will not be called for react components that are included in these pages

Adding a custom getInitialProps in your `_app.js` will disable Automatic Static Optimization in pages without Static Generation.
