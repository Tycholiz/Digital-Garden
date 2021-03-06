
# CLI
## listen
- from the CLI, we can listen for events that happen on the stripe API, and have those events forwarded to us.
- by default, `listen` will listen to the events happening on our live configuration (endpoint), found in Stripe dashboard. If we want to test, then we can listen for events happening at a different endpoint, with `stripe listen --forward-to localhost:5000/webhook`

## trigger
- from the CLI, we can fake an event happening (ie. fake a call to Stripe's API)
- if we are listening in another terminal, then we should see the fakes command being listened to.

## payment intents
### create
```
stripe payment_intents create \
  --amount=2000 \
  --currency=cad \
  -d "payment_method_types[]"=card
```
[ref](https://stripe.com/docs/api/payment_intents/create)
