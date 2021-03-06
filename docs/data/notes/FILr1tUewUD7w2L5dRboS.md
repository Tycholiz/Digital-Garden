
## Serializing/Deserializing
```
passport.serializeUser(function(user, done) {
    done(null, user.id); 
   // where is this user.id going? Are we supposed to access this anywhere?
});

// used to deserialize the user
passport.deserializeUser(function(id, done) {
    User.findById(id, function(err, user) {
        done(err, user);
    });
});
```

- the `user.id` we pass as an arg to `done` (inside `serializeUser`) is saved in the session. We can later use it to retrieve the whole user object (`deseralizeUser`)
- `serializeUser` determined which data in the `User` object shoould be saved in the session 
	- the result of `serializeUser` is attached to the session as `req.session.passport.user`
- the `id` that is passed to `deseralizeUser` is used to find the session object that has already been stored. The key it looks for is the same key provided to the `done` funtion in `serializeUser`
	- the fetched object is attached to the request object as `req.user`
