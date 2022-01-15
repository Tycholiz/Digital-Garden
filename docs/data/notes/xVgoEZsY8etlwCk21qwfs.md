
# `?.`
Just as `??` doesn't abide by JS falsy (instead, only `undefined`/`null`), so too does the `?.` operator

Example:
```js
const eligibleRegion = publishingDetails?.salesRights?.[0]?.countriesIncluded?.map(
```
