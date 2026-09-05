
RxJS can be used with React to implement streams as state. That is, we use RxJS streams as our application state.
- a stream represents events or changing values over time. This can be represented as state in React. 

React is a [[pull-first system|data#push-vs-pull-data-creation,1]], while RxJS is push-first. By making React push-first, we gain several benefits:
- Performance: only those entities that depend on the value that has changed will update, and it can be done without having to make comparisons or detect changes.
- state management becomes more declarative, in a way that can be read top-to-bottom.

## Resources
- [RxJS-React Docs](https://react-rxjs.org/docs/core-concepts)