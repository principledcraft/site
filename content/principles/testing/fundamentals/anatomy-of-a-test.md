---
title: Anatomy of a test
type: docs
weight: 1
---

Most well-structured tests follow the same three-phase pattern: set things up, do something, check what happened. This pattern goes by several names, but the most common is Arrange, Act, Assert.

## Arrange, Act, Assert

```python
def test_applying_discount_reduces_total():
    # Arrange: set up the objects and data you need
    order = Order(items=[Item("Shirt", price=50.00)])
    discount = Discount(percent=20)

    # Act: perform the action you're testing
    order.apply_discount(discount)

    # Assert: verify the result
    assert order.total == 40.00
```

Arrange prepares the state your test needs: creating objects, inserting database records, whatever the test depends on.

Act is the single action you're testing. If you find yourself doing multiple things here, you're probably testing more than one behaviour, and it's worth splitting into separate tests.

Assert checks the result. Did the function return the right value? Did the database row get updated? If the assertion fails, you want it to be obvious what went wrong.

## Why this structure matters

When a test with clear phases fails, you can immediately see what was set up, what action triggered the failure, and what expectation wasn't met. That's surprisingly hard to do when setup, action, and verification are tangled together:

{{< tabs >}}
{{< tab name="Tangled" >}}
```python
def test_order_processing():
    order = Order()
    order.add_item(Item("Shirt", price=50.00))
    assert len(order.items) == 1
    order.apply_discount(Discount(percent=20))
    assert order.total == 40.00
    order.add_item(Item("Socks", price=10.00))
    assert order.total == 50.00
```
{{< /tab >}}
{{< tab name="Separated" >}}
```python
def test_applying_discount_reduces_total():
    order = Order(items=[Item("Shirt", price=50.00)])

    order.apply_discount(Discount(percent=20))

    assert order.total == 40.00


def test_adding_item_after_discount_uses_original_price():
    order = Order(items=[Item("Shirt", price=50.00)])
    order.apply_discount(Discount(percent=20))

    order.add_item(Item("Socks", price=10.00))

    assert order.total == 50.00
```
{{< /tab >}}
{{< /tabs >}}

The tangled version tests multiple behaviours in one go. When it fails, you have to read through the whole thing to figure out which assertion broke and why. The separated version tests one thing at a time. When it fails, the test name alone tells you what went wrong.

## Keep the phases visible

Some teams leave blank lines between Arrange, Act, and Assert to make the phases visually obvious. Others use comments. The mechanism doesn't matter much. What matters is that someone reading the test can see the three parts without tracing through the logic.

The one thing to avoid is hiding important setup where the reader won't look: base classes, fixtures buried in a conftest three directories up, or setup methods that run before every test. If understanding the test requires reading four other files, the structure isn't helping anyone.
