
The app object denotes the Express application.

The `app` object has methods for:
- Routing HTTP requests
    - ex. `app.get()`, `app.param()`
- Configuring middleware
    - `app.route()`
- Rendering HTML views
    - `app.render()`
- Registering a template engine
    - `app.engine()`

### Express variables
- we can set and get variables that are available throughout express with `app.set` and `app.get`
- Express has the concept of the "app settings table", which is essentially a list of key-value pairs for configging Express that we can manipulate with `app.set`

### Local properties
The `app.locals` object has properties that are local variables within the application.
Once set, the value of `app.locals` properties persist throughout the life of the application, 
- can be accessed on `req.app.locals`

in contrast with res.locals properties that are valid only for the lifetime of the request.
