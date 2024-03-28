
Purity and immutability between components is critical. Mutability within a component is fine, as long as that mutation doesn't affect state/props.
- ex. pushing onto an array within a component is fine.

When React is interpreting JSX, it will treat custom components (`<Form>`) as recursive functions, and regular elements (`<form>`) as the finality of the chain
- ex. We say "React, render a `<Sidebar>` for me". React responds "ok what's in a `<Sidebar>`?", and it will go on like this until React gets to the html elements and it has therefore built up the entire component tree.

A component that receives another component as props will always re-render, since [[JSX|react.lang.jsx]] syntax always produces a new immutable React component

## Re-rendering
React components will re-render for various reasons:
- Props values changed: When a component's parent passes new props to it, the component will re-render. React will compare the new props with the previous props to determine if the component needs to update.
- State changed: When the `setState` function is called in a component, it triggers a re-render. 
- If a parent component re-renders, all of its child components will also re-render. 

### Higher-order components
Higher-Order Components are generic functions, as they only care about the argument being a component. The type of the component’s properties is not important.
