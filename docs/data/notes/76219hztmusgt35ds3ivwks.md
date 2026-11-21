
A Context allows us to manage the global state of a sub-tree of components
- ex. perhaps we have an ApplicationForm page with many fields, and we want to store that state in a form.

If your goal with Context is only to avoid having to pass props down many levels, then consider using [[inversion of control|general.patterns.IoC]] instead (bake those props into the component at the top level, then just pass the enhanced component itself down as a prop)

if multiple providers of the same context are wrapping a component, the innermost provider's value is what will be consumed:
```tsx
const MyContext = React.createContext();

function App() {
  return (
    <MyContext.Provider value="outer">
      <MyContext.Provider value="inner">
        <MyComponent />
      </MyContext.Provider>
    </MyContext.Provider>
  );
}

function MyComponent() {
  const contextValue = useContext(MyContext);
  console.log(contextValue) // "inner"
  return <div>{contextValue}</div>;
}
```


## Components
The React Context API consists of:
- The context
- Provider
- Consumer

### The Context
This is the state. This is the part that becomes available to consumers under the provider

```ts
// the type in the generic is the shape of the context state.
const StagingNuggetContext = createContext<>(
  undefined,
)
```

### Provider
The Provider is a component that wraps a subtree, allowing components lying underneath to consume the context state.
- it does this by taking a `value` prop which is the state (ie. the context) that will be made available to the consumers
```tsx
<StagingNuggetContext.Provider value={state}>
  {children}
</StagingNuggetContext.Provider>
```

The state we pass to `value` is our underlying state-management mechanism.
- ex. we could use a simple `useState` hook for our context state-management, or we could get a little more complex and use `useReducer`
```tsx
export function StagingNuggetProvider({ children }: StagingNuggetProviderProps) {
  const [state, dispatch] = useReducer(reducer, initialState)
  const value = { state, dispatch }
  return (
    <StagingNuggetContext.Provider value={value}>
      {children}
    </StagingNuggetContext.Provider>
  )
}
```

### Consumer
A consumer is a component that subscribes to state from the context.
- A context may have many consumers

All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes

#### Hook (optional)
In the past, we would have made a `<MyContext.Consumer>` and wrapped any components that needed to access the context. In modern React, we can just use a hook to consume it:

```ts
export const useStagingNugget = () => {
  const context = useContext(StagingNuggetContext)

  if (context === undefined) {
    throw new Error('useStagingNugget must be used within a StagingNuggetProvider')
  }
  return context
}
```

* * *

### How re-renders are handled
When a context value changes, it triggers an update cycle for all components that consume from that context. 
- This means any component that directly accesses the context value (via `useContext`, `Context.Consumer`, or the `contextType` property) will be notified of the update. Through tree reconciliation, React will then determine if a re-rendering is necessary.

*Update Cycle:* All components consuming the context will go through an update cycle when the context value changes. This means React will check if the component needs to be re-rendered.
  - If a component that consumes context is deeply nested within the provider and the context value changes, the nested component will go through an update cycle.

*Re-render:* Whether a component actually re-renders (i.e., whether its output changes and needs to be re-drawn in the DOM) depends on whether the new context value causes a change in the component's output. React's virtual DOM comparison (reconciliation) determines if a re-render is necessary.

[more info](https://stackoverflow.com/questions/50817672/does-new-react-context-api-trigger-re-renders)

### Rule of Thumb: Should you use context?
If you need to make a couple of values that don't update often available to other components then context is what you want.

If you have non-trivial global state that updates frequently, requires complex updates and is used in lots of places then you should use a state management library (like [[Redux|redux]] Zustand, or Jotai).