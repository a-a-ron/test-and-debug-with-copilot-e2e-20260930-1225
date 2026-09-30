# Debugging notes

## What the code does

`split_bill` validates that the number of people is a positive integer, converts the
money and percentage inputs to `Decimal`, and rejects negative values. It calculates
the tip, rounds the resulting total to cents, divides that rounded total equally,
rounds one share to cents, and returns that same share once for each person.

## Baseline evidence

I ran `python3 -m pytest -q`; all 7 visible tests passed. This establishes that zero
subtotals, invalid group sizes, and negative subtotal or tip inputs behave as the
current tests require, but it does not establish that rounded shares preserve the
rounded total.

## Hypothesis

Rounding a single quotient and repeating it for every person can make the sum of the
shares differ from the rounded bill total. A test splitting `10.00` among 3 people
would disprove this hypothesis if the returned shares sum to exactly `10.00`; I
expect three `3.33` shares instead, whose sum is only `9.99`.
