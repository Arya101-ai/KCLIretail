---
test: ../complete-checkout-and-confirm-the-order-receipt-header_test.md
status: failed
started: 2026-09-11T09:20:05.503Z
duration_s: 220
session_id: 02b2cc43-6627-4fee-8fde-6fa1841edad2
---

# Complete checkout and confirm the order receipt header outcome — Result

## Step 1 ✓ passed (63.4s)
md5: 984319c22e783da1f6fa8266cac89638
Open https://practicesoftwaretesting.com in a fresh browser session; from the homepage catalog, search for "Pliers", open the "Combination Pliers" product detail page, and add the item to the cart.

## Step 2 ✗ failed (145.5s)
md5: 3141bb2e45d525335a8b7bc1d7dff477
Reason: Final verification failed: "billing fields and payment fields are shown" — bug verdict: Checkout verification conflates billing and payment stages [automation_bug/agent_misstep, confidence 0.95]
On the Cart page for the current session, start checkout and move into the checkout step that collects buyer details, then assert billing fields and payment fields are shown.

## Step 3 ⏭ skipped
