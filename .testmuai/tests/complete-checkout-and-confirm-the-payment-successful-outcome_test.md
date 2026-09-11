---
assurance:
  id: t-2
  base: sha256:dd08f5569544becc56bdcdae0f0364c54d160088d45793d4f9c59e61a8d616c4
---
# Complete checkout and confirm the "Payment successful" outcome

> Prove a shopper can proceed from the Cart page into billing and payment entry, submit payment, and complete checkout with the confirmation variant that shows "Payment successful".

## Step 1

Open https://practicesoftwaretesting.com in a fresh browser session; from the homepage catalog, search for "Pliers", open the "Combination Pliers" product detail page, and add the item to the cart.

## Step 2 @verifies ac-5, ac-6

On the Cart page for the current session, start checkout and move into the checkout step that collects buyer details, then assert billing fields and payment fields are shown.

## Step 3 @verifies ac-3, ac-4, ac-7

In the checkout form, enter {{billing_first_name}}, {{billing_last_name}}, {{billing_address}}, {{billing_postcode}}, {{billing_city}}, {{billing_state}}, {{billing_country}}, {{billing_email}}, {{payment_cardholder_name}}, {{payment_card_number}}, {{payment_expiry}}, and {{payment_cvv}}, submit the payment, then assert an order confirmation screen is shown and the page contains text "Payment successful".
