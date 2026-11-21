
A callback is a function you provide as an argument to another function. When that other function has completed its task, it will execute the provided function. In the meantime, however, the code coming after the request will be executed regularly.
- spec: we see that `bcrypt.genSalt` takes in salt as its argument. given its position, this tells us that the function it is a part of will ultimately return that item. looking at the inner one, `hash` is now in that position. that function `bcrypt.hash` returns a hash.
```js
bcrypt.genSalt(10, (err, salt) => {
    if (err) return next(err)
    bcrypt.hash(user.password, salt, (err, hash) => {
      if (err) return next(err)
      user.password = hash
      next()
    })
  })
```

This is also the same value that will be used as the parameter taken in with `.then` when working with promises
```js
  bcrypt.genSalt(10)
    .then(salt => {
      return bcrypt.hash(user.password, salt)
    })
    .then(hash => {
      user.password = hash
      next()
    })
    .catch(err => {
      console.error(err)
      return next(err)
    })
```

A callback is a function that happens after we call another function, but the catch is, they’re coupled.
- However, being coupled in this way doesn’t mean we can’t reuse the callback function.

Callback hell -like structures like below can be seen as a pattern similar to piping. See the same callback-hell code written with async-await:
```js
// callbacks
readFile(request.fileToRead, data => 
  writeFile(request.fileToWrite, data, status =>
    response.send(status)
  )    
);

// async-await
const readFileData = await readFile(request.fileToRead);
const writeStatus = await writeFile(request.fileToWrite, readFileData);
response.send(writeStatus);
```
