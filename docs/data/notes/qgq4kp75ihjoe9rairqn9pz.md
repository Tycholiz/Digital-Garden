
### Async map
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
