
Benefit of a `do...while` over a regular `while` is that `do...while` will always run at least once. A `while` will only run as long as the condition is satisfied. 

Therefore,
```js
do {
    do_work();  
} while (condition);

// is equivalent to:
do_work();

while (condition) {
    do_work();
}
```

### Easy retry process with do-while
```js
// each time the loop runs, it checks to see if the place equals the address. If did, then there is an automatic retry done, accomplished with the do-while
VillageState.random = function(parcelCount = 5) {
  let parcels = [];
  for (let i = 0; i < parcelCount; i++) {
    let address = randomPick(Object.keys(roadGraph));
    let place;
    do {
      place = randomPick(Object.keys(roadGraph));
    } while (place == address);
    parcels.push({place, address});
  }
  return new VillageState("Post Office", parcels);
};
```
