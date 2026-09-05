
represent the customer's payment instruments
- they can be either:
	1. included as part of the payment intent when we are creating (so we know which credit card to use)
	2. saved to a stripe customer object for future payments

### Types
- *CARDS*	- Cards are linked to a debit or credit account at a bank. To complete a payment online, customers enter their card information at checkout.
- *WALLETS*	- Wallets are linked to a card or bank account, but can also store monetary value. Wallets typically require customer verification (e.g., biometrics, SMS, passcode) to complete a payment.
	- ex. Apple pay, Google pay
- *BANK DEBITS* -	Bank debits pull funds directly from your customer’s bank account. Customers provide their bank account information and typically agree to a mandate for you to debit their account.
- *BANK REDIRECTS* - Bank redirects add a layer of verification to complete a bank debit payment. Instead of entering their bank account information, customers are redirected to provide their online banking credentials to authorize the payment.	No, but Stripe supports recurring for some methods by converting to direct debit
- *BANK CREDIT TRANSFERS* -	Credit transfers allow customers to push funds from their bank account to yours. You provide customers with the bank account information they should send funds to.
- *BUY NOW, PAY LATER* - Buy now, pay later is a growing category of payment methods that offers customers immediate financing for online payments, typically repaid in fixed installments over time.
- *CASH-BASED VOUCHERS* -	With cash-based vouchers, customers receive a scannable voucher with a transaction reference number that they can then bring to an ATM, bank, convenience store, or supermarket to complete the payment in cash.
