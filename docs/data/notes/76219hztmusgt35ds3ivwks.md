
A Context allows us to manage the global state of a sub-tree of components
- ex. perhaps we have an ApplicationForm page with many fields, and we want to store that state in a form.

If your goal with Context is only to avoid having to pass props down many levels, then consider using [[inversion of control|general.patterns.IoC]] instead (bake those props into the component at the top level, then just pass the enhanced component itself down as a prop)

## Components
The React Context API consists of:
- The context
- Provider
- Consumer

### The Context
This is the state. This is the part that becomes available to consumers under the provider

```ts
// the type in the generic is the shape of the context state.
const StagingNuggetContext = createContext<{ state: State; dispatch: Dispatch } | undefined>(
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
  return <StagingNuggetContext.Provider value={value}>{children}</StagingNuggetContext.Provider>
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