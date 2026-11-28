
## State flow
- [Interactive demo](https://docs.swmansion.com/react-native-gesture-handler/docs/fundamentals/states-events#state-flows)

## State
Every gesture can be treated as a [[state machine|general.patterns.state-machine]]. That is, each handler instance has its own assigned state that can change when new touch events occur.

A gesture can be in one of 6 possible states
1. `undetermined`
2. `failed`
3. `began`
4. `cancelled`
5. `active`
6. `end`

## Events
There are three types of events:
1. `StateChangeEvent` - sent every time a gesture moves to a different state
2. `GestureEvent` - sent every time a gesture is updated
3. `PointerEvent` - carries information about raw touch events, like touching the screen or moving the finger.

## Composing Gestures
- [docs](https://docs.swmansion.com/react-native-gesture-handler/docs/fundamentals/gesture-composition)

Composing gestures means determining how multiple gestures interact with each other.

The `Gesture` object provides 3 methods to do this:
1. `Race` - The first gesture to have an active state will cancel the rest of the gestures on deck
  - ex. in Tinder, you can swipe left or right on a picture, but if you long press then you'll 'Heart' the profile.
2. `Simultaneous` - All of the gestures can be active at the same time
3. `Exclusive` - Only one of the gestures can be active, with the first one having higher priority than the second.