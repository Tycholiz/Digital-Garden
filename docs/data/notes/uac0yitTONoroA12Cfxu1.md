
### Sort array of numbers (asc)
```js
const sorted = [...positions].sort((a, b) => a - b)
```

### Sort array of strings (alphabetically)
```js
list.sort(function (a, b) {
    return a > b ? 1 : -1;
})
```

### Sort by multiple properties
```js
list.sort((a, b) => {
    if (b.ratings !== a.ratings) {
        // first sort by ratings (desc)
        return b.ratings - a.ratings
    } else {
        // then sort by id (desc)
        return b.id - a.id
    }
})
```

### Sort by date (as string)
```js
array.sort((a,b) => new Date(b.date).getTime() - new Date(a.date).getTime();
);
```