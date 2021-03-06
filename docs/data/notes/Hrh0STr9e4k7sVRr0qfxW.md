
There should be exactly one payment_intent for each order or customer session so we can reference the PaymentIntent later to see the history of payment attempts for a particular session.
- The PaymentIntent API tracks a payment, from initial creation through the entire checkout process, and triggers additional authentication steps when required. Therefore, a payment intent has different states during its lifetime.

When you use a PaymentIntent to collect payment from a customer, Stripe creates a charge behind the scenes

- The best practice is to create a PaymentIntent as soon as the purchase amount is known
	- ex. this might be when the customer begins the checkout process. This would be fine, since we can always update the amount if the user backs out, adds another item to their cart, then begins checkout again
- We should store the `PaymentIntentID` in our database, to be associated with the customer's shopping cart (order). If this isn't feasible, then we can store it on their session.
	- This would allow us to track and failed payment attempts for a given cart (or session)
- We should provide an `idempotency key` to the PaymentIntent
	- Doing this will help us avoid creating duplicate PaymentIntents for the same purchase
	- this key would be an ID associated with the cart/session, which we store in the database
- The PaymentIntent contains a client secret key which is unique to the individual PaymentIntent. In other words, a client secret uniquely identifies a payment intent.
	- This client secret is used as a parameter when calling `stripe.confirmCardPayment` on the client.
	- We retrieve the client secret from the PaymentIntent on our server, which we then can pass to the client. There are different approaches to getting the client secret to the client side.
- The client secret of a paymentintent is dependent on the paymentintentid.
	- An example of the secret is `pi_1IR0JiKQXfStDK4vk3nikCNH_secret_GAWuc4MSiWoezicYMqMe5qgWj`, showing that the client secret would change if the payment intent was different.
- If the customer leaves the CheckoutSession incomplete, it will expire and cancel the PaymentIntent automatically after 24 hours.

## Refunds
- We can provide an `amount` parameter to the Refunds API. Otherwise, the default amount will be the exact amount paid in the initial payment intent
