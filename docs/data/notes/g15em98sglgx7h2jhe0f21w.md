
A render prop is a prop of the signature: `(data) => JSX` (ie. a function that returns JSX)

Render props allow us to communicate with any rendered component without having to couple implementation details.
- we do this by passing data to the render prop as arguments

With render props, we enable the child component to communicate with its parent before actually rendering

A component with a render prop takes a function that returns a React element and calls it instead of implementing its own render logic.

```tsx
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

## Enhancement
The traditional way of enhancing components is to have a component with the enhancement logic, and have it wrap another component, thereby giving that child component access to the enhancements. Render props flips this model on its head, and instead we have the component that needs access to the enhancements render the component providing those enhancements:

For example, a common implementation is to provide a component with the ability to be dismissed. A naive implementation would be to have a `Dismiss` component which adds some dismissal logic, then renders the children it was passed:

```tsx
const Dismiss = ({ children }) => {
  const dismiss = () => {
    // ...code to implement dismissal animations etc
  }

  return (
    <div>
      {children}
      <button onClick={dismiss}>dismiss</button>
    </div>
  )
}

const Notification = () => {
  return (
    <Dismiss>
      { /* ...code */ }
```

The problem with the above implementation is that:
- the `dismiss` function can only be called from the `Dismiss` components
- we can't pass data to `children`

```tsx
const Dismiss = (props) => {
  const dismiss = () => {
    // dismissal animations etc.
  }

  return props.render(dismiss)
}

const Notification = () => {
  return (
    <Dismiss render={
      dismiss => <Content dismiss={dismiss} />
    } />
  )
}
```

With this render prop implementation, we are no longer hard-coding the rendered JSX in the `Dismiss` component. Instead, now it is accepting the JSX to be rendered via props, and can render by simply calling `props.render()`.

This implementation gives us flexibility to pass data to the render prop function, meaning `Dismiss` can communicate with the component being rendered, and vice versa.

### Implementing Higher-Order Components
render props also allow us to implement HOCs with minimal effort.

```jsx
const withDismiss = (Component) =>
  (props) => (
    <Dismiss>
      {(dismiss) => (
        <Component dismiss={dismiss} {...props} />
      )}
    </Dismiss>
  )
```

## E Resources
- https://blog.logrocket.com/react-reference-guide-render-props/