
### Sorted indexes
Given an array, sort the elements from the largest to the smallest. Create a result array using the indices of the original array. 
- Example: arr = [4,4,4,10,6,6,5]
- res = [3,4,5,6,0,1,2]

### Missing results from graphql query
Given a graphql-like query string and an object, return an array of paths that exist in the graphql-like string and don't exist in the object.
- Example
given:
```
{
    age
    name {
        first
        last
    }
    parent {
        name {
            first
            last
        }
        age
    }
    contact {
        phone
        email
    }
}
```

and 
```js
{
    name: {
        first: 'joe'
    },
    contact: {
        email: 'joe@joe.joe',
        phone: 7788713377
    }
}
```

Should return
```ts
[
    'age',
    'name.last',
    'parent.name.first',
    'parent.name.last',
    'parent.age',
]
```
