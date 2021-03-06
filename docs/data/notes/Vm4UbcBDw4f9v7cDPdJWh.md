
# Webhook
A webhook is a callback living on a 3rd party server that sends an HTTP request upon some event.
- ex. Twilio providing an Express server with SMS as they arrive to the Twilio server
Webhooks are a way for an app to provide other applications with real-time information
- ex. with Twilio, whenever their servers receieve an SMS, they will call the webhook endpoint that we provide to them. their webhook is a POST request, that will match up with a POST route that we define on our app server.
- The key difference with a webhook is the use of callbacks. In traditional APIs, we manually have to poll for data in order to get a real-time experience. 
- Webhooks can be thought of as "reverse APIs", as they give you the API spec, and you must design the API for them to use.
	- Put another way, the webhook makes an API request to your app, which you must handle.
- To make the connection, the webhook provider (ex. Twilio) must be provided with the address that it will send the requests to.
	- Therefore, your app must have a publicly accessible url for the webhook to work (ie. localhost will not work)

Metaphorically, webhooks are like a phone number that Stripe calls to notify you of activity in your Stripe account
- The webhook endpoint is the person answering that call who takes actions based upon the specific information it receives.

A webhook on our server says "hey, I want you to call this number (`/webhook` url) when *this* event happens"
