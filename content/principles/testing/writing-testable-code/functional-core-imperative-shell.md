---
title: Functional core, imperative shell
type: docs
weight: 3
---

This pattern takes the idea from [dependency rejection](/testing/design-and-architecture/managing-dependencies/) to its logical conclusion: split your application into a pure core that contains all the interesting logic and a thin shell that handles all the I/O.

## The structure

The **functional core** is made up of pure functions. They take data in and return data out. No database queries, no API calls, no file writes. Given the same inputs they always return the same outputs. This is where your business rules live.

The **imperative shell** is the thin layer that talks to the outside world. It reads from databases, calls APIs, writes files, and then passes the results to the core for processing. After the core returns its decision, the shell carries it out.

```python
from enum import Enum

class ShippingTier(Enum):
    STANDARD = "standard"
    EXPRESS = "express"


# Functional core: pure logic, easy to test
def decide_shipping_tier(order_total, destination_country, available_methods):
    if destination_country != "US":
        eligible = [m for m in available_methods if m.international]
    else:
        eligible = available_methods

    if order_total >= 100 and any(m.tier == ShippingTier.EXPRESS for m in eligible):
        return ShippingTier.EXPRESS
    return eligible[0].tier if eligible else ShippingTier.STANDARD


# Imperative shell: I/O and wiring
def handle_checkout(order_id):
    db = get_database()
    order = db.get_order(order_id)
    methods = db.get_shipping_methods()

    tier = decide_shipping_tier(order.total, order.country, methods)

    db.set_shipping_tier(order_id, tier)
    notify_warehouse(order_id, tier)
```

## Why this helps testing

Testing the core is trivial. No mocks, no fakes, no setup beyond creating the input data:

```python
def test_international_orders_exclude_domestic_shipping():
    methods = [
        ShippingMethod(ShippingTier.STANDARD, international=False),
        ShippingMethod(ShippingTier.EXPRESS, international=True),
    ]

    result = decide_shipping_tier(50, "DE", methods)

    assert result == ShippingTier.EXPRESS


def test_large_domestic_orders_get_express():
    methods = [
        ShippingMethod(ShippingTier.STANDARD, international=False),
        ShippingMethod(ShippingTier.EXPRESS, international=False),
    ]

    result = decide_shipping_tier(150, "US", methods)

    assert result == ShippingTier.EXPRESS
```

These tests are fast, deterministic, and isolated. They don't break when you change the database schema or switch API providers. They test exactly the logic you care about and nothing else.

The shell is harder to test, but it's also simpler. It's just wiring: fetch data, call the core, save results. A couple of integration tests verifying that the wiring works are usually enough.

## The testing advantage, quantified

Most bugs live in business logic, not in I/O wiring. If 80% of your logic is in the pure core, 80% of your tests can be pure unit tests: fast, reliable, no dependencies. The remaining 20% gets a smaller number of integration tests that verify the plumbing works.

Compare that with an architecture where business logic and I/O are mixed throughout. Every test needs either real dependencies (slow, flaky) or mocks (brittle, coupled to implementation). The total testing effort goes up while the confidence goes down.

## Limitations

This pattern works well for logic-heavy code where decisions can be separated from their effects. It's less natural when:

- **Performance matters.** Separating reads from logic from writes can mean extra data copies or passes over the data. If you're processing millions of records, you might need to interleave I/O and computation for efficiency.

- **The logic is mostly I/O.** Some code is fundamentally about orchestrating external calls: fetching data from one API, transforming it slightly, posting it to another. There isn't much pure logic to extract. Forcing the pattern here creates artificial indirection without much testing benefit.

- **State is highly interactive.** Real-time systems, game loops, or UI code where decisions depend on rapidly changing external state don't fit neatly into "fetch everything, decide, then act."

Even in these cases, you can usually find some logic worth extracting into pure functions. A game's damage calculation can be pure even if the game loop isn't. A data pipeline's transformation step can be pure even if the reading and writing can't be separated.

The pattern isn't all-or-nothing. Any logic you can pull into the core is logic you can test cheaply.

## See it in action

[Scott Wlaschin](https://fsharpforfunandprofit.com/)'s talk is where I first learned about this pattern, and it remains one of the best walkthroughs of it in practice.

&nbsp;

{{< youtube P1vES9AgfC4 >}}
