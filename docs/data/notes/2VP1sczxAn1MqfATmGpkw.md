
Morgan is a HTTP request logger middleware for Node.js.

### Token
When morgan logs to the console, the structure of the content that gets logged in determined by the tokens used

A standard way to use morgan is to use the preset tiny:
`app.use(morgan('tiny'))`, which is equivalent to:
`morgan(':method :url :status :res[content-length] - :response-time ms');`
- The part following `:` is the token.

#### Custom Tokens
We can create our own tokens with `morgan.token(nameOfToken, callback)`, where the callback returns the value that will stand in for the token name.

Tokens can be configured to accept custom arguments:
```js
app.use(morgan(':method :host :status :param[id] :res[content-length] - :response-time ms'));

morgan.token('param', function(req, res, param) {
    return req.params[param];
});
```

Morgan can be combined with Winston to great effect
