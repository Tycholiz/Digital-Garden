
All authorization should be handled by the business logic layer in the application, not in graphql
- [source](https://graphql.org/learn/authorization/)

In a REST API, authentication is often handled with a header, that contains an auth token which proves what user is making this request. Express middleware processes these headers and puts authentication data on the Express request object. 

There are two broad ways of handling authentication in GraphQL APIs:
1. **Authentication via the GraphQL server**: All users have to be logged in by the GraphQL server before they can query the endpoint—purely GraphQL workflows. Authentication is implemented in the GraphQL Schema
	- It involves first getting a JWT token, then passing that token in subsequent requests
	- this is the method proposed by Postgraphile (a JWT-based approach). 
2. **Authentication via a Web Server** (ex. Express and Passport): Users can make queries to the GraphQL endpoint once they are logged in. Authentication is implemented with middleware.
