
## Integrating Stripe
There are different ways to integrate Stripe into your application.

### Only Client-side handling
Stripe allows you to integrate a Checkout mechanism, where you only need to implement client code to perform payments.

Here is a reference for one time payments with only client-side code: https://stripe.com/docs/payments/checkout/client

### Client-side and Server-side
But there are sometimes usecase where client and server need to communicate with each other before/while a payment process (https://stripe.com/docs/payments/integration-builder). For example if you don't manage your inventory with Stripe you will need some own logic on the server. One reason would be that you just want to offer Stripe just as a payment gateway for credit cards besides other payment methods like PayPal.

### Webhooks
Well webhooks can be used in both cases. Webhooks allow you to let Stripe communicate with your backend server to inform you about succeeded payment, failed payments, customer updates, orders, billings and so on. By defining a webhook URL you can specify which events you want to receive from Stripe. You can then use events to update specified data in your database.
