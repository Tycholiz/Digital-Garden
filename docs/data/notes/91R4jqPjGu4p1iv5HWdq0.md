
***[Controlled Component](https://dev.to/stanleyjovel/simplify-controlled-components-with-react-hooks-23nn)*** - components within a form whose state is controlled with `setState`, rather than the built-in functionality provided by HTML forms
- ie. an HTML `input` that receives its `value` property from a prop in the component
```js
const ControlledInput = ({ value, eventHandler }) => (
	<input value={value} onChange={eventHandler} />
)
```
- It means that when the user types the letter “J” on the input, what is visible is not the same “J”, it may be an identical “J” that comes from the state, or whatever the event handler has put in there.
- Controlled components are the highly recommended approach
