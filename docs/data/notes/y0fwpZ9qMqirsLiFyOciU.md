
Ngrok will create a secure tunnel on the local machine from a given port (ex. `8000`) to a url hosted on the internet at their domain (ex. `e2210e647fe4.ngrok.io`)
- when a request somewhere on the internet hits an endpoint of that remote url, ngrok will forward that request on through the tunnel to the local machine.
	- ex. Stripe sends a webhook post request to `e2210e647fe4.ngrok.io:8000/webhooks/stripe`. Ngrok sees this, and passes it along to `localhost:8000/webhooks/stripe`, where the `/webhooks/stripe` endpoint defined in the application server can then handle the request
