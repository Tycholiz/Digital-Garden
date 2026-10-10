
### NextPage
We can type them using the `NextPage` type.
```js
const Page: NextPage<Props> = (props: Props) => (
  <main>Your user agent: {userAgent}</main>
)
```

## API
### `getInitialProps`
Enables SSR, allowing us to do initial data population.
- ie. sending the page with the data already populated from the server.
- carries SEO-related benefits

This method can be used to asynchronously fetch some data, which then gets passed through `props`
- The first time this method runs, it will be on the server, each time the user navigates to a different route (using `next/link` or `next/router` components), `getInitialProps` will be run on the client.

`getInitialProps` is being migrated away from in favor of `getStaticProps` or `getServerSideProps`.
