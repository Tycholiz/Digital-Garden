
## What is it?
A ref is a mutable value that is shared by all component renders.
- Think of a ref as a piece of state that doesn't abide by the same re-rendering rules that apply to normal state.
- a ref can be a function or an object

The `useRef()` hook helps create mutable variables inside a functional component that won’t update on every render.

## Why use it?
A ref should be used when:
1. the "React-way" of interacting with children (ie. Passing props) doesn't cut it 
2. we want to manually access DOM nodes or React elements created in the render method

Traditionally, refs were meant to let you directly access an html element.
- ex. focus an input field
- use refs To manage focus, text selection, or media playback

## How does it work?
Imagine we had 2 counters: one made with state, and the other with refs. As expected, when we increment the state count, the component understands that it needs to re-render, so it does so and reads in the new value of count. When we increment the ref count however, the component understands that it should not update, so we end up still seeing the old value, even though underneath the hood, the count has still risen. Since nothing has caused the component to re-render, this value will remain static. Now imagine that we increment the state count. React will recognize that an updated state means that it needs to re-render the component. Upon doing so, the current value of the ref will be read.

- The variables created with `useRef()` persist in between component renders (like state).
- The nature of refs is that we can change the value of the ref, and it will not cause the component to re-render.
- Refs have a value called `.current()`, which gives us the value of the ref in the current rendering of the component.
- Most elements inside of your document have a ref attribute, which facilitates the use of useRef to reference elements inside of your HTML.
- refs created using createRef are not persisted between re-renders. A new ref is always created whenever the component is re-rendered.
- Updating a ref value is considered a side effect

This is the reason why you want to update your ref value in event handlers and effects and not during rendering

- ex. Imagine we have a list of users. When we click one, it logs out the username after 1 second. Now imagine that we click on a user, then click on another before the `console.log` occurs.
In this scenario, class components would log out user2, while functional components would log out user1.
- This shows that functional components attach the call made to the user selected at the time. If we want to make a functional component that behaves like a class, we can use a ref, which is a value that is shared between all renders (in this sense it's sort of like an instance variable) <-- (in retrospect I think this should read "static variable", since it exists across all renders, similar to how static variables exist across all instances of that class; however this information came from React docs)

### Forward Ref
- What if we want to take a **ref** that is currently existing in the component and pass it down the tree?
	- ie. we can reference a component rendered by `<Child />` from `<Parent />`
	- ex. we have a `<Form />` and `<App />`. We have a ref defined in `App` and want to attach it to an input box inside `Form`. We wrap `Form` with `forwardRef`, enabling the component to receive the `ref`. in `Form`, we pass the ref to the input. Now, we can control that component from `App`.
- the component that has `forwardRef` in it is the component that is receiving the ref (ie. the child)
- `forwardRef()` will create a component, accepting 2 arguments: `props` and `ref` (a regular component just accepts props)

#### Example
Imagine we have a CheckoutPage component which renders a PaymentForm. The CheckoutPage handles all logic related to network calls in the app, and PaymentForm handles all form logic, including managing its own state. This is a reasonable separation of concerns. However, what happens if we want to access a piece of state in the parent from the child? Imagine the network call our parent made needed to get the address from the form. The "React Way" of accomplishing this is to bring the form state up one level to live in the parent component. However, this breaks our separation of concerns pattern. We can use an escape hatch forward ref in this circumstance.

Illustration:
```js
const Parent = () => {
    const myRef = useRef();
    return <Child ref={myRef} />;
}

const Child = React.forwardRef((props, ref) => {
    const [myState, setMyState] = useState('This is my state!');
    useImperativeHandle(ref, () => ({getMyState: () => {return myState}}), [myState]);
})
```
Then you should be able to get myState in the Parent component by calling: `myRef.current.getMyState()`

#### Combined ref
- for low-level UI development, it's common to want to use both a local ref and an external one (`forwardRef`).

### UER
[Dan Abramov on refs](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)
