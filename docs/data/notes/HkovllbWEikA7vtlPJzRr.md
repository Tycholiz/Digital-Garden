
purity and immutability between components is critical. mutability within a component is fine.
- ex. pushing onto an array within a component is fine.

When React is interpreting JSX, it will treat custom components (`<Form>`) as recursive functions, and regular elements (`<form>`) as the finality of the chain
- ex. We say "React, render a `<Sidebar>` for me". React responds "ok what's in a `<Sidebar>`?", and it will go on like this until React gets to the html elements and it has therefore built up the entire component tree.

### Higher-order components
Higher-Order Components are generic functions, as they only care about the argument being a component. The type of the component’s properties is not important.
