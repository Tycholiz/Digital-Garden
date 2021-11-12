
### Using a class to manage state
Consider the [Robot delivery game](https://eloquentjavascript.net/07_robot.html#p_md/LJiyP4s), where we have a robot going around delivering parcels to different villages. For state, we need to track:
1. Robot's current location
2. The array of parcels (inc. current location, and destination)

To accomplish this, a class `VillageState` was used, which looks like this:
```js
VillageState {
  place: 'Post Office',
  parcels: [ { place: 'Post Office', address: "Alice's House" } ]
}
```

In taking this approach to state management, the next question to ask is: "what methods should exist that will result in a new state to be computed?". The answer here is the action of the robot moving from location to location, so we add a `move` method. The signature of this function is:
- taking in the desired destination, and returns the new `VillageState`.

Taking this approach lets us think of our program in terms of "turns". If two people were competing to see who could deliver all of the parcels in less turns, then conceptually we can think of each new computation of state (ie. an instance of `VillageState`) as being a turn.
```js
class VillageState {
  constructor(place, parcels) {
    this.place = place;
    this.parcels = parcels;
  }

  move(destination) {
    if (!roadGraph[this.place].includes(destination)) {
      return this;
    } else {
      let parcels = this.parcels.map(p => {
        if (p.place != this.place) return p;
        return {place: destination, address: p.address};
      }).filter(p => p.place != p.address);
      return new VillageState(destination, parcels);
    }
  }
}
```