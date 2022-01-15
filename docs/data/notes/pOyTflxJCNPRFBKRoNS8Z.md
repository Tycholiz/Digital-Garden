
# @requires
When extending a schema, it's often because you want to expose data from Node.js that would be too difficult (or impossible) to access from PostgreSQL
- in other words, we come to a situation where we don't want to rely on Postgres to process and give us data, but instead want to rely on Node to do the job for us.

## USD to CAD example
- Imagine in our database we store the price of a product in USD. However, we want to be able to query the `priceInCad`, directly as a field on the graphql schema:
```
query Product {
	product {
		priceInCad
	}
}
```

We can accomplish this by making a custom Postgraphile plugin, which would extend the Graphql schema to include the `priceInCadCents` field on the `Product` type:
```
const MyForeignExchangePlugin = makeExtendSchemaPlugin(build => {
  return {
    typeDefs: gql`
			extend type Product {
				priceInCadCents: Int! @requires(columns: ["price_in_us_cents"])
			}`,
    resolvers: {
      Product: {
        priceInCadCents: async product => {
          // Note that the columns are converted to fields, so the case changes
          // from `price_in_us_cents` to `priceInUsCents`
          const { priceInUsCents } = product;
          return await convertUsdToCad(priceInUsCents);
        },
      },
    },
  };
});
```

we include the `@require` directive to show that in order to calculate this field, we need the postgres column `price_in_us_cents`
