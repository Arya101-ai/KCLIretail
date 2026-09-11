---
test: ../complete-checkout-and-confirm-the-payment-successful-outcome_test.md
status: failed
started: 2026-09-11T09:24:00.453Z
duration_s: 201
session_id: 092a8b65-0ca6-4ee5-b636-60776a7b8462
---

# Complete checkout and confirm the "Payment successful" outcome — Result

## Step 1 ✓ passed (59.1s)
md5: 984319c22e783da1f6fa8266cac89638
Open https://practicesoftwaretesting.com in a fresh browser session; from the homepage catalog, search for "Pliers", open the "Combination Pliers" product detail page, and add the item to the cart.

## Step 2 ✗ failed (134.1s)
md5: 3141bb2e45d525335a8b7bc1d7dff477
Reason: Final verification failed: "billing fields and payment fields are shown" — bug verdict: Checkout assertion expects billing fields after advancing to payment [automation_bug/state_transition_bug, confidence 0.93]
On the Cart page for the current session, start checkout and move into the checkout step that collects buyer details, then assert billing fields and payment fields are shown.

## Step 3 ⏭ skipped
