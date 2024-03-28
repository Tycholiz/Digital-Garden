
Prefer `useReducer` over `useState` when either:
1. The state is more complex than a scalar or simple object.
2. the next state depends on the previous state
3. you are storing an array in state and the user can edit each item in the array.
4. you have components with many state updates spread across many event handlers
  - this gets simplified because we can consolidate all the state update logic outside your component in a single function, called "reducer". The event handlers become concise because they only specify the user "actions"

`useReducer` also lets you optimize performance for components that trigger deep updates because you can pass dispatch down instead of callbacks.

### Throw and error on `default`
There is little point in the default to a switch statement to simply return `state`:
```js
default:
  return state;
```

This is like allowing a silent error to pass through, as actions should be explicit. We want to be notified of any non-existent actions and typos immediately, so we throw an error.

### Example: useUndo
```js
import * as React from 'react'

const UNDO = 'UNDO'
const REDO = 'REDO'
const SET = 'SET'
const RESET = 'RESET'

function undoReducer(state, action) {
  const {past, present, future} = state
  const {type, newPresent} = action

  switch (action.type) {
    case UNDO: {
      if (past.length === 0) return state

      const previous = past[past.length - 1]
      const newPast = past.slice(0, past.length - 1)

      return {
        past: newPast,
        present: previous,
        future: [present, ...future],
      }
    }

    case REDO: {
      if (future.length === 0) return state

      const next = future[0]
      const newFuture = future.slice(1)

      return {
        past: [...past, present],
        present: next,
        future: newFuture,
      }
    }

    case SET: {
      if (newPresent === present) return state

      return {
        past: [...past, present],
        present: newPresent,
        future: [],
      }
    }

    case RESET: {
      return {
        past: [],
        present: newPresent,
        future: [],
      }
    }
    default: {
      throw new Error(`Unhandled action type: ${type}`)
    }
  }
}

function useUndo(initialPresent) {
  const [state, dispatch] = React.useReducer(undoReducer, {
    past: [],
    present: initialPresent,
    future: [],
  })

  const canUndo = state.past.length !== 0
  const canRedo = state.future.length !== 0
  const undo = React.useCallback(() => dispatch({type: UNDO}), [])
  const redo = React.useCallback(() => dispatch({type: REDO}), [])
  const set = React.useCallback(
    newPresent => dispatch({type: SET, newPresent}),
    [],
  )
  const reset = React.useCallback(
    newPresent => dispatch({type: RESET, newPresent}),
    [],
  )

  return {...state, set, reset, undo, redo, canUndo, canRedo}
}

export default useUndo
```