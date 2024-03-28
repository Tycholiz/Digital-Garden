
### Filter on multiple factors
```js
return restaurants
    .filter(resto => {
        if (veganFriendly && resto.veganFriendly === false) {
            return false
        }
        if (maxPrice < resto.price) {
            return false
        }
        if (maxDistance < resto.distance) {
            return false
        }
        return true
```