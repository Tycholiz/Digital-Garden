
## Misc
with (spec) stripe elements, we can use the `stripe_code` on the client as a way to get the credit card information (presaved) into the UI.

By default, anyone can POST data to our webhook. This is why we have the webhook secret to protect us, and only react to the POST request once we have verified that it is coming from Stripe
- each webhook secret is associated with a single webhook endpoint

### Credit Cards Internationally
Regions like Europe and India require our application to handle requests from banks to authenticate a purchase (3DS or OTP)
- If we are using webhooks, then the authentication is already handled and we don't have to worry about this.
