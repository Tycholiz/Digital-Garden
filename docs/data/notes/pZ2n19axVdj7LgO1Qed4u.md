
### Sorted indexes
Given an array, sort the elements from the largest to the smallest. Create a result array using the indices of the original array. 
- Example: arr = [4,4,4,10,6,6,5]
- res = [3,4,5,6,0,1,2]

### Missing results from graphql query
Given a graphql-like query string and an object, return an array of paths that exist in the graphql-like string and don't exist in the object.
- Example
given:
```js
const graphqlQuery = `
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
}`
```

and 
```js
const person = {
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

### Find path through data
```js
// function(string): string
function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
}

const testData = [
    { "source": "Home", "destination": "Home Cleaning" },
    { "source": "Home Cleaning", "destination": "Restaurants" },
    { "source": "Restaurants", "destination": "Delivery" },
    { "source": "Delivery", "destination": "Address Search" },
    { "source": "Address Search", "destination": "Burgers" },
    { "source": "Burgers", "destination": "Order Delivery" },
    { "source": "Order Delivery", "destination": "Start Order" },
    { "source": "Start Order", "destination": "Turkey Burger" },
];

shuffle(testData);

// Given the shuffled click data and an origin page, find the final destination page
// ex: input: 'Home' -> output: 'Turkey Burger'
function getDestinationFromOrigin(origin) {
    
    const initialSourceObject = testData.filter(el => el.source === origin)[0]
    
    const history = []
    let currentSource = initialSourceObject.source
    let currentDestination = initialSourceObject.destination
    history.push(currentDestination)
 
    // stay in while loop while currentDestination does not exist as a source in the inputArray
    while (testData.map(el => el.source).indexOf(currentDestination) !== -1) {

        // find element where source = currentDestination
        currentSource = currentDestination
        // find element of array where source = currentSource, then get the destination of that object and set it to currentDestination
        currentDestination = testData.find(el => el.source === currentSource).destination

        if (history.includes(currentSource)) {
            console.log('hey, you\'re in a loop!')
            return
        }
        history.push(currentDestination)
    }
    return currentDestination
}
// what if we get an input origin that doesn't exist?
// what if we get a circular path?
// what if a single source has multiple destinations? (ie. multiple objects with same source but different destination)
```