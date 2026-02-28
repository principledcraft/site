---
title: Practical test tips
type: docs
weight: 2
---

Good tests aren't just about testing the right things. How you write them matters too. A well-named, well-structured test is easier to maintain, easier to debug, and more useful as documentation.

## Name tests for what they verify

A test name should describe the behaviour being checked, not the method being called. When a test fails in CI, the name is often the first (and sometimes only) thing you read.

{{< tabs >}}
{{< tab name="Weak names" >}}
```python
def test_calculate_shipping():
    ...

def test_calculate_shipping_2():
    ...

def test_edge_case():
    ...
```
{{< /tab >}}
{{< tab name="Descriptive names" >}}
```python
def test_shipping_is_free_when_order_exceeds_threshold():
    ...

def test_shipping_uses_flat_rate_below_threshold():
    ...

def test_shipping_raises_error_for_negative_total():
    ...
```
{{< /tab >}}
{{< /tabs >}}

The descriptive names tell you exactly what broke without opening the test file. They also act as a specification: reading the test names gives you a summary of how shipping calculation is supposed to work.

## Use parameterized tests to reduce duplication

When you're testing the same behaviour with different inputs, parameterized tests let you avoid copy-pasting the same test structure over and over.

{{< tabs >}}
{{< tab name="Duplicated" >}}
```python
def test_shipping_free_at_50():
    assert calculate_shipping(50.00) == 0

def test_shipping_free_at_75():
    assert calculate_shipping(75.00) == 0

def test_shipping_free_at_100():
    assert calculate_shipping(100.00) == 0
```
{{< /tab >}}
{{< tab name="Parameterized" >}}
```python
@pytest.mark.parametrize("order_total", [50.00, 75.00, 100.00])
def test_shipping_is_free_above_threshold(order_total):
    assert calculate_shipping(order_total) == 0
```
{{< /tab >}}
{{< /tabs >}}

Parameterization works well when the test logic is identical and only the data changes. If the setup or assertions differ between cases, separate tests are clearer.

## Keep test setup independent

Each test should create its own state from scratch. When tests share mutable state, they become order-dependent: test A passes alone but fails after test B, or the whole suite breaks when you run tests in parallel.

{{< tabs >}}
{{< tab name="Shared state" >}}
```python
cart = Cart()  # shared across tests

def test_add_item():
    cart.add(Item("Shirt", 50))
    assert len(cart.items) == 1

def test_cart_total():
    # depends on test_add_item having run first
    assert cart.total == 50
```
{{< /tab >}}
{{< tab name="Independent" >}}
```python
def test_add_item():
    cart = Cart()
    cart.add(Item("Shirt", 50))
    assert len(cart.items) == 1

def test_cart_total():
    cart = Cart()
    cart.add(Item("Shirt", 50))
    assert cart.total == 50
```
{{< /tab >}}
{{< /tabs >}}

Yes, the independent version has some duplication. That's fine. The duplication is a few lines; the debugging session when shared state causes a mysterious failure costs hours.

## The coverage trap

100% line coverage feels reassuring, but it can be misleading. A line being executed during a test does not mean it was meaningfully verified.

Consider a function that takes three boolean parameters. That function has eight possible input combinations, but a single test case might execute every line while only covering one combination. Line coverage reports 100%. Seven paths remain untested.

This is why stricter coverage models exist. Branch coverage checks whether both sides of every conditional were evaluated. Modified condition/decision coverage (MC/DC) goes further, verifying that each condition independently affects the outcome. These models are standard in safety-critical industries like aviation and medical devices, where a missed path can cost lives.

When teams chase line coverage as a target, the incentives go wrong. People write tests to satisfy the metric rather than to catch real problems. Coverage is better used as a tool for spotting gaps than as a number to hit.

## Risk-based testing

You cannot test everything exhaustively, and you shouldn't try. A payment processing module deserves more thorough testing than a logging formatter. A function with complex branching logic needs more test cases than a simple data transfer object.

Think about what happens if something breaks in production. If the answer is "customers lose money" or "data gets corrupted," invest heavily in testing that area. If the answer is "a tooltip shows the wrong colour," lighter coverage is fine.

This doesn't mean ignoring low-risk code entirely. It means being deliberate about where you spend your testing effort.

## Avoid over-abstracting test code

It's tempting to DRY up test code the same way you would production code. Shared fixtures, helper functions, base test classes, factory methods. But test code and production code have different priorities. Production code optimises for avoiding duplication. Test code optimises for readability.

When a test fails, you want to understand it by reading that one test, not by chasing through a chain of helpers and fixtures. A bit of repetition in tests is a feature, not a bug. It keeps each test self-contained and easy to understand.

If you do create test helpers, keep them close to where they're used and make them obvious. A helper called `make_order(items=...)` is fine. A base class that sets up half the test state in a `setUp` method three inheritance levels up is not.
