---
title: Test behaviour, not implementation
type: docs
weight: 1
---

The single most common mistake in testing is coupling tests to implementation details. When tests depend on how code works internally rather than what it does, every refactoring breaks them, even when the actual behaviour hasn't changed.

## What does "behaviour" mean?

A behaviour is an observable outcome: a return value, a state change, a message sent. An implementation detail is how that outcome is produced: which internal methods get called, what data structures are used, how the algorithm works.

{{< tabs >}}
{{< tab name="Testing implementation" >}}
```python
def test_calculate_total_uses_reduce():
    order = Order(items=[Item("Shirt", 50), Item("Socks", 10)])

    # This test knows about internal implementation
    assert order._subtotals == [50, 10]
    assert order._reduction_method == "sum"
```
{{< /tab >}}
{{< tab name="Testing behaviour" >}}
```python
def test_calculate_total_sums_item_prices():
    order = Order(items=[Item("Shirt", 50), Item("Socks", 10)])

    assert order.total == 60
```
{{< /tab >}}
{{< /tabs >}}

The first test breaks if you change the internal representation. The second only breaks if the actual total calculation is wrong. You could rewrite the entire internals of `Order` and the second test wouldn't care.

## The refactoring litmus test

If you can refactor the internals of your code without changing any observable behaviour, and your tests break, those tests are coupled to implementation. Good tests survive refactoring. That's the whole point of having them.

A practical rule that follows from this: never change your tests and your implementation at the same time. If you're doing both, you've lost your safety net. Either write the test first and then change the code, or change the code under existing tests and then update tests if the *behaviour* has actually changed.

## London vs Classical schools

There's an ongoing debate about what a "unit" in unit testing actually means. [Vladimir Khorikov](https://enterprisecraftsmanship.com/) covers this in depth in [Unit Testing: Principles, Practices, and Patterns](/books/unit-testing-principles-practices-and-patterns/).

The **London school** (also called the mockist approach) isolates every class by mocking all its collaborators. A unit is a single class. Every dependency gets replaced with a test double.

The **Classical school** (also called the Detroit approach) lets units collaborate naturally and only mocks external dependencies like databases or APIs. A unit is a unit of behaviour, which might involve several classes working together.

The London approach makes it easy to pinpoint which class broke, but the heavy mocking means tests are tightly coupled to the interaction between classes. Rename a method, change a call order, extract a helper, and your tests break even though the behaviour is identical.

The Classical approach gives you more freedom to refactor, because your tests don't know or care how many classes are involved. The tradeoff is that when a test fails, you might have to look at a larger area of code to find the problem.

In practice, the Classical approach tends to produce tests that are more resilient to refactoring, which is the property you most want from a test suite. The London approach has its uses, but if you find yourself writing more mock setup than actual assertions, that's worth questioning.

## When tests break for the wrong reasons

If every small refactoring causes a cascade of test failures, the test suite is working against you instead of for you. Some signs to watch for:

- Tests that verify method calls rather than outcomes
- Tests that mirror the implementation step by step
- Tests that break when you extract a private method or rename an internal variable
- Assertions about the order of operations rather than the final result

These are all symptoms of testing implementation rather than behaviour. The fix usually isn't to write "better" tests for the same structure. It's to step back and ask: what does this code actually *do* from the outside? Test that.
