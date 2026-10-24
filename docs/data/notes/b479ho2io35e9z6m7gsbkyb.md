
### Check if key exists
```js
usersByBusiness.has(review.businessId)
```

### Append to an existing `Set`
```js
usersByBusiness.get(review.businessId)?.add(review.userId)
```

### Create new set (value) if doesn't exist; append if it does
```js
if (!usersByBusiness.has(review.businessId)) {
    usersByBusiness.set(review.businessId, new Set<number>())
}
usersByBusiness.get(review.businessId)?.add(review.userId)
// { 2 => { 101, 103}, 3 => { 101, 102, 103 }, 4 => { 101 } }
```

### Iterating a Map
```js
for (const [key, value] of myMap) {
  console.log(`${key} = ${value}`);
}
```