

At a high level, cursor-based pagination works like this:
1. User requests some data (either through initial page load, or clicking some UI pagination button).
2. Along with the list of data that to display, we also want to get pagination information `pageInfo` that is defined on the connection

```gql
query OrganizationForLearningReact {
  organization(login: "the-road-to-learn-react") {
    name
    url
    repository(name: "the-road-to-learn-react") {
      issues(first: 10, after: "Y3Vyc29yOnYyOpHODAESqg==") {
        pageInfo {
          endCursor
        }
        edges {
          node {
            author {
              login
            }
          }
        }
      }
    }
  }
}
```
3. Get the `id` of the last item in the most previously-fetched data list (known as the `cursor`) 
  - stored in `endCursor` variable
4. Store the cursor in local state (e.g. React, Apollo Client)
5. pass as variable on subsequent fetches of data (ex. when user clicks "Load more", or scrolls more on infinite page)
6. use that variable as input to the `after` parameter defined on the Connection (ex. in looking through a list of Github issues, the cursor is defined on the `IssuesConnection`)

[[Edges|graphql.structures.edge]] enable us to perform cursor-based pagination. Since the cursor is (usually) just an `id`, we can use the `after` argument on a list, passing that `id`. This allows us to specify the starting point that data should be retrieved from