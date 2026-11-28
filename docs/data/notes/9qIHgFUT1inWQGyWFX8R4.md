
Cloudflare Workers KV provides access to a secure low latency key-value store at all of the data centers in Cloudflare's global network
- usage ex. save the cart and checkout in Workers KV and when our webhook is notified by Stripe of a successful payment_intent, we create the order in our backend.
	- ex. this decouples your server having to be up and running from being able to process orders
