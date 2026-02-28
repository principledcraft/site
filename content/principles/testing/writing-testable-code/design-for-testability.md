---
title: Design for testability
type: docs
weight: 1
---

When testing feels painful, the problem is usually the code, not the tests.

Code that's hard to test tends to have the same structural problems: too many responsibilities in one place, hidden dependencies, tight coupling to external systems. Making it testable means fixing those problems, which is why testable code and well-designed code look so similar.

## Testing as a design feedback mechanism

Difficulty writing a test is a signal. If you can't test a function without standing up a database, configuring three services, and mocking two more, the function is doing too much. The test is telling you about a design problem.

This is one of the most practical benefits of writing tests alongside your code. You find out immediately when something is too tangled. Without tests, these problems stay hidden until someone needs to change the code months later and discovers it's nearly impossible to understand in isolation.

## What makes code hard to test?

A few common patterns:

**Hidden dependencies.** A function that creates its own database connection or reads from a global config is hard to test because you can't substitute those dependencies. The function works fine in production but is locked to its environment.

```python
def get_user_orders(user_id):
    db = DatabaseConnection()  # hidden dependency
    return db.query("SELECT * FROM orders WHERE user_id = ?", user_id)
```

**Mixed concerns.** A function that fetches data, applies business rules, and sends an email is hard to test because you can't check one piece without dealing with the others.

```python
def process_refund(order_id):
    db = DatabaseConnection()
    order = db.get_order(order_id)
    if order.total > 100:
        refund_amount = order.total * 0.9  # business rule
    else:
        refund_amount = order.total
    payment_gateway.refund(order.payment_id, refund_amount)
    email_service.send(order.customer_email, f"Refund: {refund_amount}")
    return refund_amount
```

Testing the refund calculation here requires setting up a database, a payment gateway, and an email service. The interesting logic (the 90% rule for large orders) is buried inside.

**Tight coupling.** When classes call each other's internals or share mutable state, you can't test one without bringing along the other. Everything is tangled together and nothing works in isolation.

## What makes code easy to test?

The opposites of the problems above:

**Explicit dependencies.** Pass dependencies in rather than creating them internally. This is [dependency injection](/testing/design-and-architecture/managing-dependencies/), and it makes code testable almost by definition.

**Separated concerns.** Keep business logic separate from I/O. If the refund calculation is its own function, you can test it with plain values:

```python
def calculate_refund(order_total: float) -> float:
    if order_total > 100:
        return order_total * 0.9
    return order_total


def test_large_orders_get_partial_refund():
    assert calculate_refund(150.00) == 135.00

def test_small_orders_get_full_refund():
    assert calculate_refund(50.00) == 50.00
```

No database, no payment gateway, no email. Just inputs and outputs.

**Small surface area.** Functions that do one thing are easy to test because the setup is minimal and the assertions are straightforward.

## The tradeoff

Designing for testability adds some indirection. Instead of one function that does everything, you end up with several that each do one thing. That's more files, more function calls, and more wiring to connect them.

For most code, this tradeoff is worth it. The code becomes easier to understand, change, and test, which saves far more time than the indirection costs.

Where it gets less clear-cut is in performance-sensitive paths or very simple scripts where the overhead of splitting things apart doesn't pay for itself. A 50-line data migration script probably doesn't need dependency injection and separated concerns. A payment processing service does.
