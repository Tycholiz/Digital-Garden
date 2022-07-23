
Unlike the traditional frameworks (React and Vue) which carry out the bulk of their work in the browser, Svelte shifts that work into a compile step that happens when an app is built.

# Features of the framework
## Updates to the DOM
When you update component state in Svelte, it doesn't update the DOM immediately. Instead, it waits until the next microtask to see if there are any other changes that need to be applied, including in other components. Doing so avoids unnecessary work and allows the browser to batch things more effectively.
- sometimes this behavior is undesirable, and for that we can turn to `tick()` lifecycle method

## Reactivity in Svelte
Svelte's reactivity is triggered by assignments (ie. setting a new variable, or changing the assignment of an existing variable)
- therefore `map`, `filter` and `reduce` all trigger re-renders, because those methods return a new array. `push` would not trigger a re-render, unless we reassign the same variable to equal itself:
```js
function addNumber() {
	numbers.push(numbers.length + 1);
	numbers = numbers;
}
```
The idiomatic way of course is just to use non-mutative methods.

Updating properties of an object will also cause the re-render, since assigning new values to keys of an object is considered to be assignment.

### Reactive declarations
Some parts of a component's state need to be computed from other state , and therefore need to be computed when its dependent values change 
- eg. fullname computed from firstname+lastname. If firstname changes, then fullname needs to be recomputed.

For these circumstances, we can use reactive declarations, which look like:
```js
let firstname = 'joe';
let lastname = 'schmidt';
$: fullname = firstname + lastname;
```

`$: ` basically says, "do this thing (e.g. set fullname equal to firstname + lastname) whenever the dependent variables involved are updated (ie. whenever firstname or lastname change)". 
- In other words, this symbol marks a statement as reactive.

This symbol is known as the "destiny operator" in reactive programming.
- A destiny operator ensures a variable is updated whenever values that it's computed from are changed)

Reactive values become particularly valuable when you need to reference them multiple times, or you have values that depend on other reactive values.

We can also just run arbitrary code blocks that are executed any time a dependent variable changes:
- ex. here, any time firstname or lastname changes, the codeblock is run
```js
$: {
    console.log(`Nice to meet you!`)
    console.log(`my fullname is ${firstname} ${lastname}`)
}
```

We can even use conditionals:
```js
$: if (count >= 10) {
	alert(`count is dangerously high!`);
	count = 9;
}
```

## Props
Declare props in the `<script>` tag:
```js
<script>
	export let answer;
    export let name = 'Kyle'; // defaultValue
</script>
```

Props are passed (almost) identically to React
```js
<Nested answer={42}/>
```

## DOM Logic
### Conditional rendering
```js
{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}
```

### Loop
```js
<ul>
	{#each cats as { id, name }} // destructuring here
		<li><a target="_blank" href="https://www.youtube.com/watch?v={cat.id}">
            ID: {id}
			Name: {name}
		</a></li>
	{/each}
</ul>
```

### Data fetching
Svelte makes it easy to await the value of  directly in your markup:
- Only the most recent promise is considered, meaning you don't need to worry about race conditions.

```js
{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

## Events
More or less in the same manner as React, any event can be listened to with the `on` directive:
```html
<div on:mousemove={handleMousemove}>
	The mouse position is {m.x} x {m.y}
</div>
```

### Event modifiers
DOM event handlers can have modifiers that alter their behaviour. 
- ex. a handler with a once modifier will only run a single time:
```html
<button on:click|once={handleClick}>
	Click me
</button>
```

Notable modifiers:
- `preventDefault`
- `once` - remove the handler after first time it runs
- `self` - only trigger handler if `event.target` is the element itself

### Event dispatching
A component can be set to dispatch events by creating an event dispatcher and calling them via a handler. The parent component (here `App`) can listen to messages dispatched from a child component via the `on:message` directive (where `message` is the event name we are dispatching).
- Without this `on:message` attribute, messages would still be dispatched, but the App would not react to it.

inner.svelte:
```html
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

App.svelte:
```html
<script>
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Inner on:message={handleMessage}/>
```

### Event forwarding
Unlike DOM events, component events don't [[bubble|js.event-loop.event-bubbling]]. If you want to listen to an event on a deeply nested component, the intermediate components must *forward* the event.

This would be the mid-layer, with a dispatched event called in the `Inner` component. Here, `on:message` is what bubbles the event up to the parent
```html
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message/>
```

### Binding
Though Svelte takes a top-down data flow approach like React, it can be useful to break that paradigm, and can be done so with bindings.

we can use the bind:value directive:
```html
<input bind:value={name}>
```

This means that not only will changes to the value of `name` update the input value, but changes to the input value will update `name`.

Checkbox:
```html
<input type=checkbox bind:checked={yes}>
```

Radio:
```html
// `scoops` is an array of choices
<input type=radio bind:group={scoops} name="scoops" value={1}>
```

## Lifecycles
Like React, Svelte has a familiar (albeit simpler) list of lifecycle methods.
`onMount`, `onDestroy`, `onUpdate`, `beforeUpdate`, `afterUpdate`

### tick
`tick` is a lifecycle method distinct from the familiar ones. It's different in that it can be called any time. It returns a promise that resolves as soon as pending state changes have been applied to the DOM.

## Stores
Global state solution of Svelte. 
A store is simply an object with `subscribe` method, allowing interested parties to be notified whenever the value changes.
```js
// stores.js
import { writable } from 'svelte/store';

// this is a writable store
export const count = writable(0);

/* * * * * * * * * * * * * * * * * * * */
// Incrementor.svelte
import { count } from './stores.js';

function decrement() {
    count.update(n => n - 1);
}
```

## Motions, Animations and Transitions
Svelte provides lots of motion, animation and transition support out-of-the-box:

Examples: 
- https://svelte.dev/tutorial/tweened
- https://svelte.dev/tutorial/spring
- https://svelte.dev/tutorial/transition

## Actions
Actions are essentially element-level lifecycle functions. They're useful for things like:

- interfacing with third-party libraries
- lazy-loaded images
- tooltips
- adding custom event handlers

Svelte actions allows you to build code in response to the lifecycle of DOM elements

[Make an on-screen object pannable](https://svelte.dev/tutorial/actions)

## Context API
The context API provides a mechanism for components to 'talk' to each other without passing around data and functions as props, or dispatching lots of events.

### Parts to it
[source](https://svelte.dev/tutorial/context-api)
There are 2 halves to the API: `setContext` and `getContext`.
- if a component calls `setContext(key, context)`, then any child component can retrieve that context with `getContext(key)`

Unlike React Context, you do not need to import and wrap your subtree with the Provider in order to get access to the context. Simply, the parent component calls `setContext` and that context is available to all children to want access to it.

* * *

## CSS Class shorthand
```html
<button
	class="{current === 'foo' ? 'selected' : ''}"
	on:click="{() => current = 'foo'}"
>foo</button>
```

Can be shortened to:
```html
<button
	class:selected="{current === 'foo'}"
	on:click="{() => current = 'foo'}"
>foo</button>
```

## Component children
We can render children of a component like React by passing content between a component's tags.

In React, it looks like this:
```js
const childComponent = ({ children }) => {
    return (
        <div>
            {children}
        </div>
    )
}
```

In Svelte, it looks like this:
```html
<div>
	<slot></slot>
</div>
```

Fallbacks can be specified by placing data between the `<slot>` tags. Think of this like a default value to the children "prop". This is called a *default slot*d
- [named slots](https://svelte.dev/tutorial/named-slots) can also be used for more control over placement.
