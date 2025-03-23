+++
title = "Price Adjustment Calculator"
date = "2025-03-22"
+++

# The Problem

Imagine you're a business owner who recently started accepting credit & debit
cards. The company processing these payments keeps a small percentage (5%) of
transaction you submit to them. Your current price is \\$10, and you want to
find a new price that will still net you the same amount. First, you try
increasing your prices by the same percentage that the processor witholds.

- New Price: \\$10.00 × 105% = \\$10.50
- Processing Fee: \\$10.50 × 5% = \\$0.52
- Revenue \\$10.50 - \\$0.52 = \\$9.98

Hmm... you're two cents short. 5% of \\$10.00 is less than 5% of \\$10.50, so
that makes sense. At this point, you remember that you want to charge sales tax,
as well, which makes things even worse.

- New Price: \\$10.00 × 105% = \\$10.50
- Sales Tax: \\$10.50 × 10% = \\$1.05
- Processing Fee: \\$11.55 × 5% = \\$0.58
- Revenue: \\$11.55 - \\$1.05 - \\$0.58 = \\$9.92

Now you're eight cents short and can feel a headache starting... But then you
get an email! It's from your payment processor informing you of a new fixed
\\$0.05 fee added to each transaction _on top_ of the percentage fee. Oh, and
your product is packaged in cans which means they incurr an additional \\$0.10
flat tax in addition to sales tax. At this point, you decide to just raise your
price by a dollar to try and offset these fees because you simply can't be
bothered to figure it out on your own.

- New Price: \\$10.00 + \\$1.00 = \\$11.00
- Taxes: \\$11.00 × 10% + \\$0.50 = \\$1.60
- Processing Fee: \\$12.60 × 5% + \\$0.05 = \\$0.68
- Revenue: \\$12.60 - \\$1.60 - \\$0.68 = \\$10.32

Now you've overshot it by \\$0.32. You could probably try a lower price instead,
but who has the time to figure that out? Whatever. The new price is \\$11.00.

This situation is especially troublesome in markets where price is the main
point of competition. Your new price of \\$11.00 could mean fewer sales because
a competitor priced their product at \\$10.75 even though you'd be willing to go
as low as \\$10.66 (the price that would net you exactly \\$10).

# Deriving a Formula

Ideally, we'd like to be able to use our desired revenue and the various fee and
tax amounts to compute our adjusted price. We can represent the revenue
transaction as a system of equations, and derive a formula for computing the
adjusted price.

## Definitions

First, Let's define the variables and equations that we'll be working with. For
brevity, percentages are represented as numbers between 0 and 1.

Variables:

- $ P $ - Price of the good
- $ T $ - Total taxes
- $ t_p $ - Percent-based tax
- $ t_f $ - Fixed tax
- $ U $ - Total processing fee
- $ u_p $ - Percent-based processing fee
- $ u_f $ - Fixed processing fee
- $ S $ - Amount submitted to payment processor
- $ R $ - Revenue from the sale

Equations:

- Taxes $ T = P t_p + t_f $
- Amount submitted $ S = P + T $
- Processing fee $ U = S u_p + u_f $
- Revenue $ R = S - T - U $

## Derivation

Now, starting with the final equation in the previous section, we can repeatedly
substitute & simplify to achieve our desired result ($P$ = something).

$$
  \begin{aligned}
    R &= S - T - U \\\\
      &= (P + T) - T - U \\\\
      &= P - U \\\\
      &= P - (S u_p + u_f) \\\\
      &= P - (P + T) u_p - u_f \\\\
      &= P - P u_p - (P t_p + t_f) u_p - u_f \\\\
      &= P - P u_p - P t_p u_p - t_f u_p - u_f \\\\
    R &= P (1 - u_p - t_p u_p) - t_f u_p - u_f \\\\
    R + t_f u_p + u_f &= P (1 - u_p - t_p u_p) \\\\
    \frac{R + t_f u_p + u_f}{1 - u_p - t_p u_p} &= P \\\\
  \end{aligned}
$$

And there we have it! An equation for $P$ in terms of $R$, $t_p$, $t_f$, $u_p$,
and $u_f$:

$$
  \boxed{P = \frac{R + t_f u_p + u_f}{1 - u_p - t_p u_p}}
$$

# A Friendlier Interface

This equation is nice, but it's a little scary for the average person to look
at, and it's not very easy to use. Manually inputting each value in a calculator
is tedious, and it's easy to forget to use parentheses for the denominator. To
make it more accessible, I developed
[a website](https://laclark.me/tools/price-adjustment-calculator) which displays
the correctly adjusted price based on the inputs to the required fields. This
was a perfect situation for me to use React, a library that I had been learning
for creating user interfaces. The above equation can very easily be translated
into TypeScript:

```TypeScript
const listedPrice =
  (revenue + fixedProcessFee + fixedTax * percentProcessFee) /
  (1 - percentProcessFee - percentProcessFee * percentTax);
```

The variables on the right side of the equation are stored as _state_ using
React's `useState` hook. When any of them changes, the `listedPrice` is
re-computed and the results are re-rendered. This allow users to instantly see
how changes in their inputs affect the results.

### TODO:

- Interpretability of results, LLMs, "Vibe Coding"
- Testing, JS number implementation
- License Choice?

# Caveats

- Fixed processing fees are usually per-transaction rather than per-item, so if
  a customer buys more than one item in a single transaction, you may
  _overshoot_ the desired revenue for that transaction. One way to adjust for
  this is to divide the fixed processing fee by the average number of items per
  transaction.
-
