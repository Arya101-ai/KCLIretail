---
assurance:
  id: t-4
  base: sha256:981e3bbb291ec749f99424f7343a0f22f5bdffec7ece18b1e4ff9dc35be8940b
---
# Add a product from its detail page and verify the cart badge and stored cart session

> Prove that a shopper can add a product from a product detail page, see the cart badge increase, and have cart data persisted in browser localStorage.

## Step 1

Open https://practicesoftwaretesting.com in a browser; from the homepage catalog, open the detail page for the first visible product card.

## Step 2

On that product detail page, store the current number shown in the top-header cart badge as baseline_badge_count.

## Step 3 @verifies ac-9, ac-10, ac-11

On the same product detail page, add the product to the cart, then assert the top-header cart badge shows baseline_badge_count + 1, DevTools `localStorage.getItem('cart')` returns a non-null value, and parsing that returned value as JSON succeeds without error.
