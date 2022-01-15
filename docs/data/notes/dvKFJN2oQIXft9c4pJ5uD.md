
A styled component can take any prop. It passes it on to the HTML node if it's a valid attribute, otherwise it only passes it into interpolated functions.

#### `attrs`
A chainable method called on a styled component, with the purpose of attaching some props to the styled component.
- The fact that it's chainable shows that calling `.attrs` on a styled component returns another styled component.

The first and only argument is an object that will be merged into the rest of the component's props

The *attributes* refer to those found on the component/HTML node in question.

##### Purpose
`attrs` is useful for defining default attributes, and defining dynamic props
- ex. you want a `<Button />` component with default attribute of `type="button"` (instead of the normal `type="submit"`)
- ex. also, if you want a default size of small, but to be able to override that.

```js
const Button = styled.button.attrs(props => ({
  // Every <Button /> will now have type="button" as default
  type: props.type || 'button'
  // small is the default size, but this can be overridden
  size: props.size || 'small'
}))`
  display: block;
  ...
`
```

#### `as`
keep all the styling you've applied to a component but just switch out what's being ultimately rendered
- ex. Keep all the stylings of your `Button` styled component, but actually render it as a `<input />` instead of a `<button />`

This is done at runtime.

This is known as a "polymorphic prop"
