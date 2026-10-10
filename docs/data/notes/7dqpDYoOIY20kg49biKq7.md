
Caddy config natively uses a JSON structure. However, because writing JSON by hand can be error-prone, we have config adapters to help us.
- The standard config adapter coming with Caddy converts a `Caddyfile` into JSON.
- At the end of the day, JSON is more flexible, but not quite as simple. If we stay on the well-beaten path, then we probably can just get by with a Caddyfile.

We can use a `caddy.json` config file like this:
```json
{
	"apps": {
		"http": {
			"servers": {
				"example": {
					"listen": [":2015"],
					"routes": [
						{
							"handle": [{
								"handler": "static_response",
								"body": "Hello, world!"
							}]
						}
					]
				}
			}
		}
	
}
```

Or, the equivalent can be expressed in a `Caddyfile`
```Caddyfile
:2015

respond "Hello, world!"
```

If we run `caddy adapt` in a directory where there is a `Caddyfile`, then we can see the json that the Caddyfile contents are being converted into. This is the *config adaptor* at work.

If we are serving multiple apps from a single Caddy server, then we need to use curly braces (`{}`) in the Caddyfile to wrap the details of a single app. Otherwise, curly braces aren't needed
