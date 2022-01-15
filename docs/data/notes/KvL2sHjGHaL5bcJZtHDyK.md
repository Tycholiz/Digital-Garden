
# Memo
- def - a HOC that memoizes the rendered output of a component
- `Memo` accepts a second arg that lets us determine whether or not the component should re-render (defaults to `true`, meaning it won't re-render)
- When deciding to update DOM, React first renders your component, then compares the result with the previous render result. If the render results are different, React updates the DOM.
	- To be clear, React will always render, but won't always update DOM
- When we wrap our component with `Memo`, the first render will be memoized. Every subsequent render will involve the usual check of oldProps/oldState vs nextProps/nextState, but instead of rendering the component *then* comparing the output to the DOM, it will just take the memoized output. The logic is: since props or state didn't change, we shouldn't need to re-render it (which may well be the case for some components; in which circumstance we should certainly wrap the component with `Memo`
- Components using hooks can be freely wrapped in React.memo() to achieve memoization
- use mostly when component trees get too wide or too deep

## When to use
1. the component is purely functional (if class, use PureComponent and modify SCU), and given same props, will always return the same component
2. the component renders often
	- often what causes this is a Parent with state/props that change often, and a Child who would receive the same props for a longer period of time. In this case, memoize the Child
		- ex. we have a `<MoviePage />` component and inside it a `<MovieInfo title={props.title} />`. MoviePage holds the state to determine how many thumbs up a movie has, and it pings the server every second. Therefore, when the number changes, the component re-renders, and thereby forces MovieInfo to render. The issue here is that `props.title` is not going to change each time, so React is needlessly rendering the component each time. 
3. the component often receives the same props in between renders
4. the component contains a decent amount of components that are subject to conditional rendering (and depend on a prop)
