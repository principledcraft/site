---
title: Managing dependencies in tests
type: docs
weight: 2
---

Most testing difficulties come down to dependencies. Your code needs a database, a file system, an external API, or some other thing that's hard to control in a test. How you handle those dependencies determines whether your tests are fast, reliable, and maintainable, or slow, flaky, and painful.

There are two broad approaches, and they sit on a spectrum: dependency injection and dependency rejection.

## Dependency injection

Dependency injection means passing dependencies into a function or class rather than letting it create them internally. It's the most common first step toward making code testable.

{{< tabs >}}
{{< tab name="Hidden dependency" >}}
```python
class OrderService:
    def get_total(self, order_id):
        db = DatabaseConnection()  # creates its own dependency
        order = db.get_order(order_id)
        return sum(item.price for item in order.items)
```
{{< /tab >}}
{{< tab name="Injected dependency" >}}
```python
class OrderService:
    def __init__(self, db):
        self.db = db  # dependency passed in

    def get_total(self, order_id):
        order = self.db.get_order(order_id)
        return sum(item.price for item in order.items)
```
{{< /tab >}}
{{< /tabs >}}

With the injected version, you can pass in a fake or stub database during testing:

```python
def test_order_total_sums_items():
    fake_db = FakeOrderStore(orders={
        1: Order(items=[Item("Shirt", 50), Item("Socks", 10)])
    })
    service = OrderService(db=fake_db)

    assert service.get_total(order_id=1) == 60
```

Dependency injection makes code testable, but it doesn't eliminate the dependency. The code still *uses* a database; it just receives it from outside. You still need some form of test double to replace it.

## Dependency rejection

Dependency rejection goes a step further. Instead of making dependencies swappable, you restructure the code so the interesting logic doesn't need dependencies at all. The term comes from [Mark Seemann](https://blog.ploeh.dk/)'s work and is discussed in [Code That Fits in Your Head](https://www.amazon.com/dp/0137464401).

The idea: push I/O and side effects to the edges. Keep the core logic pure.

{{< tabs >}}
{{< tab name="With dependency" >}}
```python
class PricingService:
    def __init__(self, discount_repo):
        self.discount_repo = discount_repo

    def calculate_total(self, order):
        discount = self.discount_repo.get_active_discount()
        return sum(i.price for i in order.items) * (1 - discount.rate)
```
{{< /tab >}}
{{< tab name="Dependency rejected" >}}
```python
def calculate_total(items, discount_rate):
    return sum(i.price for i in items) * (1 - discount_rate)
```
{{< /tab >}}
{{< /tabs >}}

The second version doesn't know about repositories. It takes plain data and returns a number. Testing it requires no mocks, no fakes, no setup:

```python
def test_discount_applied_to_total():
    items = [Item("Shirt", 50), Item("Socks", 10)]

    total = calculate_total(items, discount_rate=0.1)

    assert total == 54.00
```

The caller is responsible for fetching the discount rate from wherever it lives. The calculation doesn't care.

## The spectrum

These two approaches aren't mutually exclusive. In practice, you use both:

- **Dependency rejection** for core business logic. Keep it pure, pass in data, get results back.
- **Dependency injection** for the code that bridges between pure logic and the outside world. These components receive their dependencies so you can swap in fakes for integration tests.

The goal is to maximise the amount of code that doesn't need dependencies at all. The less code that touches the outside world, the less you need to mock, fake, or stub.

## Tradeoffs

Dependency injection is straightforward and familiar. Most developers already know the pattern, and most frameworks support it. The downside is that it can lead to constructor parameter lists that keep growing, and heavy use of test doubles.

Dependency rejection produces simpler, more testable functions, but it requires you to think differently about how you structure code. Not everything can be a pure function. At some point you need to actually read from the database and send the email. The question is how thin you can make that layer.

Both approaches are better than the alternative: code that creates its own dependencies internally and can only be tested by running the entire system.

## Further watching

Mark Seemann's talk on dependency rejection covers the reasoning behind this approach in depth:

{{< youtube xG5qP5AWQws >}}
