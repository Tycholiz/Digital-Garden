
testerooni
# Overview
Idea is that all the lower-level networking tasks (HTTP requests etc) as well as storing the data (caching) should be abstracted away and the declaration of data dependencies should be the dominant part.
- This is precisely what GraphQL client libraries like Relay or Apollo will enable you to do. They provide the abstraction that you need to be able to focus on the important parts of your application rather than having to deal with the repetitive implementation of infrastructure.

Apollo is completely separate from the view-layer, making it framework agnostic.

Apollo allows us to send queries from the view-layer

### As state management
[high-level overview](https://www.apollographql.com/blog/dispatch-this-using-apollo-client-3-as-a-state-management-solution/)

### vs Relay
Relay is more opinionated, Apollo is unopinionated

### vs URQL
[Open source alternative to Apollo](https://formidable.com/open-source/urql/)
