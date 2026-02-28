---
title: Types of tests
type: docs
weight: 2
---

Different types of tests check different things at different levels of the system. Knowing what each type is good at, and where it falls short, helps you decide which ones to write.

## Unit tests

A unit test checks a small piece of logic in isolation. What counts as a "unit" is debated (more on that in [Test Behaviour, Not Implementation](/testing/writing-good-tests/test-behaviour-not-implementation/)), but either way: test one thing, get a fast answer.

```python
def test_calculate_shipping_free_above_threshold():
    cost = calculate_shipping(order_total=75.00, threshold=50.00)

    assert cost == 0.00


def test_calculate_shipping_flat_rate_below_threshold():
    cost = calculate_shipping(order_total=30.00, threshold=50.00)

    assert cost == 5.99
```

Unit tests are fast, cheap to write, and give precise feedback when they fail. They won't tell you whether the pieces work together, though.

## Integration tests

Integration tests verify that multiple components work together. This might mean testing a function that queries a real database, or an API endpoint that calls an external service.

```python
def test_checkout_persists_order_to_database(db_session):
    cart = Cart(items=[Item("Shirt", price=50.00)])

    order = checkout(cart, db=db_session)

    saved_order = db_session.query(Order).filter_by(id=order.id).first()
    assert saved_order is not None
    assert saved_order.total == 50.00
```

Integration tests catch the problems unit tests miss: misconfigured connections, wrong query syntax, serialisation that silently drops fields. They're slower and need more setup, but they test the boundaries where most real bugs live.

## System and end-to-end tests

System tests exercise the entire application the way a user would. For a web app, that might mean driving a browser through a checkout flow. For an API, sending real HTTP requests and checking the responses.

These tests are the most realistic but also the slowest and most fragile. A single UI change can break dozens of them. Use them sparingly, for the critical user journeys.

## Acceptance tests

Acceptance tests verify that the software meets a specific requirement or user story. They overlap with system tests in scope, but the intent is different: does this feature do what the user asked for?

When written in plain language (using tools like Behave or pytest-bdd), they give developers and stakeholders a shared definition of "done".

## The testing pyramid

The testing pyramid, introduced by [Mike Cohn](https://en.wikipedia.org/wiki/Mike_Cohn) in *Succeeding with Agile*, is a simple model for thinking about how many tests to write at each level.

<img src="/images/testing-pyramid.svg" alt="The testing pyramid: unit tests at the base (fast, cheap), integration tests in the middle, and end-to-end tests at the top (slow, expensive). Cost increases upward, speed increases downward." style="max-width: 500px; margin: 1rem auto; display: block;" />

Tests at the bottom tend to be fast and cheap. Tests at the top are slower, more expensive, and more likely to break on small changes, but they give you confidence that the whole system works together. You want many more at the bottom than at the top.

What actually matters is the cost and speed of each test, not its label. If you can write an end-to-end test that runs in milliseconds and rarely breaks, it's just as valuable as a unit test. The pyramid reflects the common case, where broader tests tend to be slower and more expensive, but it's not a law. What matters is whether the test is worth maintaining, not where it sits in the pyramid.

## Picking the right mix

No single type of test covers everything. The pyramid gives you a sensible default: lots of unit tests, fewer integration tests, and a small number of end-to-end tests for the paths that absolutely cannot break.

The exact ratio depends on what your codebase actually does. A system with complex business logic needs more unit tests. A system that's mostly glue code connecting external services probably needs more integration tests than unit tests, and the pyramid shape might not apply at all.

When deciding what to write, ask: what can go wrong here, and which type of test would actually catch it?
