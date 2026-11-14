
## What is it?
A ref is a mutable value that is shared by all re-renders of a single component.
- Think of a ref as a piece of state that doesn't abide by the same re-rendering rules that apply to normal state.
    - normally this state controls imperative DOM elements, such as if an `<input>` is focused or not, or the current width of a resizable window.
- a ref can be a function or an object

Refs are for when you want a component to "remember" some information, but you don’t want that information to trigger new renders.
- The `.current` value is like a secret pocket of your component that React doesn’t track. Therefore, it is an "escape hatch" from React's one-way data flow.

The `useRef()` hook creates a mutable variable (ie. `ref.current`) inside a functional component that won’t update on every render.

When we attach a ref to a `<div>` element, we are creating a reference to that particular `<div>` element.
- under the hood, React automatically assigns the `current` property of the ref to the corresponding DOM node when the component renders.
- once we have that reference to the JSX element, we can access properties and methods of the DOM node through the ref.
    - ex. if we attach a ref to a `<div>`, we can access the `innerText` property

With refs, we can also create a reference to another React component. To do this, the component to be referenced needs to use `forwardRef`, which allows us to forward the ref prop from a child component to a DOM element or a class or functional component.

## Why use it?
A ref should be used when:
1. the "React-way" of interacting with children (ie. Passing props) doesn't cut it 
2. we want to manually access DOM nodes or React elements created in the render method

Traditionally, refs were meant to let you directly access an html element.
- ex. focus an input field
- use refs To manage focus, text selection, or media playback

When a piece of information is only needed by event handlers and changing it doesn’t require a re-render, using a ref may be more efficient.

### Use-cases
- Focus Management: Setting focus on an input field or a button programmatically, for example, in response to a specific user action.
    - ex. imagine we have a Parent component which renders 2 `<input>`. We want to enact the business logic that *upon filling out the first `<input>` and hitting enter, the second `<input>` should automatically be focused*.
- Integration with Non-React Libraries: When you need to incorporate third-party libraries that require direct DOM access or manipulation, refs can be useful.
- Measurements: If you need to measure elements, like their width, height, or position, refs can be necessary.
- Animations: While there are React libraries for animations, sometimes you might need to use refs for more granular control.

### When not to use Refs
- Overusing for State Management: component state should be managed within the confines of React's prop and state system.
    - ex. using a ref to capture and validate an input value instead of utilizing the component's state and controlled components might be seen as not idiomatic.
- Bypassing React's Reconciliation: If refs are used to make direct DOM manipulations, like changing a node's text or toggling a class, instead of relying on React's re-render mechanism, it's probably a misuse. React works best when it's allowed to handle the DOM based on its state.
- Mixing Logic: Mixing up business logic with direct DOM operations through refs can make the component harder to reason about, test, and maintain.
- Overhead: Using too many refs can make a component overly complex, harder to read, and harder to maintain. If every other element has a ref attached, it's a red flag that the code might not be optimally structured.
- Not Utilizing Available Abstractions: Sometimes developers use refs because they aren't aware of the higher-level abstractions available. 
    - ex. they might use a ref to set focus on an input field, unaware that some UI libraries provide components with built-in focus management.

## How does it work?
Imagine we had 2 counters: one made with state, and the other with refs. As expected, when we increment the state count, the component understands that it needs to re-render, so it does so and reads in the new value of count. When we increment the ref count however, the component understands that it should not update, so we end up still seeing the old value, even though underneath the hood, the count has still risen. Since nothing has caused the component to re-render, this value will remain static. Now imagine that we increment the state count. React will recognize that an updated state means that it needs to re-render the component. Upon doing so, the current value of the ref will be read.

- The variables created with `useRef()` persist in between component renders (like state).
- The nature of refs is that we can change the value of the ref, and it will not cause the component to re-render.
- Refs have a value called `.current()`, which gives us the value of the ref in the current rendering of the component.
    - this value is intentionally mutable
- Most elements inside of your document have a ref attribute, which facilitates the use of useRef to reference elements inside of your HTML.
- refs created using createRef are not persisted between re-renders. A new ref is always created whenever the component is re-rendered.
- Updating a ref value is considered a side effect

This is the reason why you want to update your ref value in event handlers and effects and not during rendering

- ex. Imagine we have a list of users. When we click one, it logs out the username after 1 second. Now imagine that we click on a user, then click on another before the `console.log` occurs.
In this scenario, class components would log out user2, while functional components would log out user1.
- This shows that functional components attach the call made to the user selected at the time. If we want to make a functional component that behaves like a class, we can use a ref, which is a value that is shared between all renders (in this sense it's sort of like an instance variable) <-- (in retrospect I think this should read "static variable", since it exists across all renders, similar to how static variables exist across all instances of that class; however this information came from React docs)

### Forward Ref
- What if we want to take a **ref** that is currently existing in the component and pass it down the tree?
	- ie. from `<Parent />`, we can reference a component rendered by its `<Child />`
	- ex. we have `<App />` and `<Form />`. We have a `ref` defined in `App` and want to attach it to an input box inside `Form`. We wrap `Form` with `forwardRef`, enabling the component to receive the `ref` from `App`. in `Form`, we pass the ref to the input. Now, we can control that component from `App`.
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


### `useImperativeHandle`
This hooks lets us customize the ref (exposed via `forwardRef`) that is exposed to the parent who passed it.
- in other words, when a parent component renders a child component that accepts a ref, the child component can then choose to customize the values/methods that should be exposed to the parent.

#### Use case #1: limiting methods exposed to parent
- ex. imagine we had a `<Parent />` and a `<Child />`. The `<Child />` renders an `<input />`. From the `<Parent />`, we define a ref and pass it to the `<Child />`, because we want to imperatively control some of the `<input />` methods from the `<Parent />`. However, we don't need access to all the methods, so instead of attaching that forwarded ref directly to the `<input />`, we use it to create an imperative handle (`useImperativeHandle`). Then, we create another ref (`inputRef`) in the `<Child />` which gets attached to the `<input />`, and we use that `inputRef` in the `useImperativeHandle`. The result is that two methods (`focus` and `scrollIntoView`) are exposed to the parent, instead of the whole reference to the `<input />`.
```jsx
const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

#### Use case #2: Combining the methods of multiple refs into one function call
- ex. Imagine we had a blog with a button which when clicked, would scroll the page down to the bottom of the comments list then highlight the `<input />`. From our top-level component, we want to be able to just call `postRef.current.scrollAndFocusAddComment()`. From the child component, what we would do is define the `useImperativeHandle` which exposes that method, and define 2 more refs that get passed down to its children:
```jsx
const Post = forwardRef((props, ref) => {
  const commentsRef = useRef(null);
  const addCommentRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      scrollAndFocusAddComment() {
        commentsRef.current.scrollToBottom();
        addCommentRef.current.focus();
      }
    };
  }, []);

   return (
    <>
      <CommentList ref={commentsRef} />
      <AddComment ref={addCommentRef} />
    </>
  );
```


### UER
[Dan Abramov on refs](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)
