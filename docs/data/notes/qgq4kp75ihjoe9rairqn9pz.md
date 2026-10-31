
### Async map
`map()` does not inherently handle asynchronous code, so when you use it with async functions, it immediately returns an array of promises, not the resolved values.
- as a result, you must wrap your async map block in `Promise.all(...)`

```js
const deleteUser = async (user) => {
    return await user.delete({
        where: { id: user.id }
    })
}
const deleteUsers = async () => {
    return Promise.all(users.map((user) => deleteUser(user)))
}

// later you can chain aync maps like this
deleteTweets().then(() => {
    deleteUsers()
})
```

```js
const combinedPatients: WaitlistPatient[] = await Promise.all(
    [...facilityWaitlistPatients, ...uniqueGeneralPatients].map(async patient => {
        return {
            ...patient,
        }
    })
)
```
