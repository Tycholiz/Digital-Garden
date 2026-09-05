
A system of modular components for GraphQL networking
	- Each link represents a subset of functionality that can be composed with other links to create complex control flows of data.
	- In a few words, Apollo Links are chainable "units" that you can snap together to define how each GraphQL request is handled by your GraphQL client.
- When you fire a GraphQL request, each Link's functionality is applied one after another, allowing you to control the request lifecycle in a way that makes sense for your application
	- ex. Links can provide retrying, polling, and batching
- At a basic level, a link is a function that takes an operation and returns an observable
	- an operation is an object with information like: query, variables, context
- links are required when creating an Apollo client instance.
- the core of a link is the `request` method, which accepts the `operation` (ex. query, mutation) as an argument
	- this method is called every time execute is run on that link chain (typically each time an operation is passed through the link chain)

## HTTP Link
- most common Apollo Link
- The http link is a terminating link that fetches GraphQL results from a GraphQL endpoint using http
	- This can be used for authentication, persisted queries, dynamic uris, and other granular updates.

## State link
- allows us to use graphql query operations on client state. 

## Auth link
- allows us to add an authorization header to each request
