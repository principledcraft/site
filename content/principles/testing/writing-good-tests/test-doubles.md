---
title: "Test doubles: mocks, stubs, and fakes"
type: docs
weight: 3
---

When code depends on something external, a database, an API, the file system, you often need to replace that dependency in tests. The replacements are called test doubles, and they come in several flavours.

## The types

A **stub** returns canned data. It doesn't verify how it was called; it just provides the answers your code needs to keep running.

```python
def test_order_summary_includes_customer_name():
    customer_repo = StubCustomerRepo(name="Alice")

    summary = generate_order_summary(order_id=1, customer_repo=customer_repo)

    assert "Alice" in summary
```

A **mock** records how it was called and lets you make assertions about those calls. It verifies interaction.

```python
def test_checkout_sends_confirmation_email():
    email_service = Mock()
    cart = Cart(items=[Item("Shirt", 50)])

    checkout(cart, email_service=email_service)

    email_service.send_confirmation.assert_called_once()
```

A **fake** is a working implementation that takes a shortcut. An in-memory database instead of PostgreSQL, a local file instead of S3. Fakes behave like the real thing but are faster and easier to set up.

```python
def test_save_and_retrieve_order():
    db = InMemoryOrderStore()
    order = Order(items=[Item("Shirt", 50)])

    db.save(order)
    retrieved = db.get(order.id)

    assert retrieved.total == 50
```

## When mocking becomes a problem

Mocks are useful in moderation. The trouble starts when tests are mostly mock setup with a thin assertion at the end.

```python
def test_process_payment():
    payment_gateway = Mock()
    payment_gateway.charge.return_value = PaymentResult(success=True)
    inventory = Mock()
    inventory.reserve.return_value = True
    email_service = Mock()
    logger = Mock()

    process_order(
        order=Order(items=[Item("Shirt", 50)]),
        gateway=payment_gateway,
        inventory=inventory,
        email=email_service,
        logger=logger,
    )

    payment_gateway.charge.assert_called_once()
    inventory.reserve.assert_called_once()
    email_service.send_confirmation.assert_called_once()
```

This test is really testing the choreography of method calls, not the behaviour of the system. If you rearrange the order of calls, or rename a method, the test breaks, even if the end result is identical. It's testing implementation, not behaviour (see [Test Behaviour, Not Implementation](/testing/writing-good-tests/test-behaviour-not-implementation/)).

Heavy mocking is often a sign that the code has too many dependencies. If a function needs four mocks to test, the function is doing too much. The right response isn't usually to write better mocks; it's to restructure the code so it needs fewer dependencies. More on that in [Design for Testability](/testing/design-and-architecture/design-for-testability/) and [Managing Dependencies](/testing/design-and-architecture/managing-dependencies/).

## Mocks can hide real failures

There's another risk with mocks that's easy to miss: a mocked test can pass while the real integration is broken. If you mock a database call and return the data your code expects, the test passes. But if the actual query has a syntax error or the schema has changed, you won't find out until production.

This is why mocks and integration tests serve different purposes. Mocks let you test logic in isolation. Integration tests verify that the pieces actually fit together. You need both, and leaning too heavily on mocks means you're only getting half the picture.

## Prefer stubs and fakes over mocks

When you have the choice, stubs and fakes tend to produce more resilient tests than mocks. They test outcomes rather than interactions. A fake in-memory database lets you verify that data was saved and retrieved correctly, without caring about which SQL query was used or how many times `commit` was called.

The general hierarchy: use the real thing when practical, a fake when the real thing is too slow or complex, a stub when you just need canned data, and a mock only when you specifically need to verify that an interaction happened.
