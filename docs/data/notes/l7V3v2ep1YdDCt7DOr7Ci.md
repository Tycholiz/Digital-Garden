
Redux uses a different instance of the store for every request.
Redux is actually based in part on [[CQRS|general.patterns.architectural.CQRS]] and [[event sourcing|general.patterns.event-sourcing]]

re-render performance in Redux is faster in comparison to Context API because every component that consumes the Context will re-render even if the state relevant to that component hasn’t changed.

# Tools
- [Redux Toolkit: Opinionated Redux](https://redux-toolkit.js.org/)
