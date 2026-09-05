
### Promisify
```js
onRequest((request, response) => {
    const promisifiedFunction = new Promise(resolve => {
        nonPromisifiedFunction(request.fileToRead, data => resolve(data));
    });
    
    promisifiedFunction
        .then(data => writeFile(request.fileToWrite, data))
        .then(status => response.send(status));
});
```

### Cancelling promises
Just reject the promise with “Cancelled” as the reason. If you need to deal with it differently than a “normal” error, do your branching in your error handler.

### `sleep()`/`waitFor()`
```js
let sleep = ms => new Promise(resolve => setTimeout(resolve, ms))

async function waitFor(fn){
    while(!fn()) await sleep(1000)
    return fn()
}

/* Usage */
// wait for an element to exist, then assign it to a variable
let bed = await waitFor(() => document.getElementById('bedId'))
if(!bed) doSomeErrorHandling();

// wait for a variable to be truthy
await waitFor(() => el.loaded)

// wait for some test to be true
await waitFor(() => video.currentTime > 21)

// add a specific timeout to stop waiting
await waitFor(() => video.currentTime > 21, 60*1000)

// send an element as an argument once it exists
doSomething(await waitFor(() => selector('...'))

// pass it some other test function
if(await waitFor(someTest)) console.log('test passed')
else console.log("test didn't pass after 20 seconds")
```

### Implementing polling with promises
ie. *“wait for x then do y”*
```ts
const waitFor = async (fn: () => Promise<boolean>, timeoutMs: number) => {
  return new Promise(async (resolve) => {
    const startTime = Date.now()

    const poll = async () => {
      if (await fn()) resolve(true)
      else if (Date.now() - startTime > timeoutMs) resolve(false)
      else setTimeout(poll, POLL_FREQUENCY_MS)
    }
    await poll()
  })
}

async function isItemInTable(eventId: string) {
  const { Item } = await dynamoDocumentClient.send(
    new GetItemCommand({
      TableName: TABLE_NAME,
      Key: marshall({ event_id: eventId }),
    }),
  )
  return !!Item
}

if (await waitFor(() => isItemInTable(eventId), TEST_TIMEOUT_MS)) {
    // ...
```
alternate: http://www.abigstick.com/2019/03/29/pollers_and_promises.html

### Setting a timeout condition with multiple promises
Imagine we want to check multiple shards simultaneously for the same piece of information. We could create a promise for each of the shards we are searching, and each promise will resolve to either true or false depending on if the item was found. In this case, we could use `Promise.race` to return as soon as one promise resolves. This also lends itself nicely to implementing a timeout functionality.
```ts
const promises = Shards.map(shard =>
    // checkShard returns a promise.
    checkShard(shard, StreamArn, responsePayloadAsString)
)

const timeoutPromise = new Promise((resolve) => setTimeout(resolve, TEST_TIMEOUT_MS))

/* if result is truthy, the record was found and the test passed. If the timer resolves first, then the test is considered to have timed out. */
const result = await Promise.race([ ...promises, timeoutPromise ])
```

### Run 2+ async functions in parallel
```js
function randomWait() {
    const seconds = Math.random() * 5;
  
    return new Promise(resolve => {
      setTimeout(
        () => resolve(`Resolved after ${seconds.toFixed(2)} seconds`),
        seconds * 1000
      );
    });
}

function fn() {
    const result1 = randomWait()
    .then(result => console.log('1st call:', result));
    const result2 = randomWait()
    .then(result => console.log('2nd call:', result));
}

fn();
```
