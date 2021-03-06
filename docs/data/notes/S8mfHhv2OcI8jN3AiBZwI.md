
## Setup Intent
- A SetupIntent guides you through the process of setting up and saving a customer's payment credentials for future payments.
- If the SetupIntent is used with a Customer, upon success, it will automatically attach the resulting payment method to that Customer.
- if we wanted to save a credit card at the time of purchase, then we can add the `setup_future_intent` at the time of creating a PaymentIntent. This negates our need to use the SetupIntent. spec: Therefore, the SetupIntent is needed only when saving a payment for later, but not making a purchase at the time (like when someone click "add payment method".
- Creating a SetupIntent will generate a PaymentMethod to be attached to a customer, which can then be used to create a PaymentIntent when you are ready to charge them.
