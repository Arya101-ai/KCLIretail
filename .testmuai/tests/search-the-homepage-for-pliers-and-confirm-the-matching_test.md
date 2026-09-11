---
assurance:
  id: t-1
  base: sha256:6dd2bc657c6e6bd947b102e0988c9824b52cd3fdf442c96f00fc78e95c6d28c5
---
# Search the homepage for "Pliers" and confirm the matching result title

> Prove that submitting the documented keyword from the homepage returns the documented matching product card and visible card title.

## Step 1

Open https://practicesoftwaretesting.com and wait on the homepage with the catalog search input available.

## Step 2 @verifies ac-1, ac-2

On the homepage, submit the keyword search Pliers from the catalog search input and observe the filtered product grid, then assert a product card titled Combination Pliers is shown and the visible card title text contains Combination Pliers.
