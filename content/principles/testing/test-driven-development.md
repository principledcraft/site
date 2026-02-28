---
title: Test-driven development
type: docs
weight: 5
---

Test-driven development (TDD) inverts the usual order: write the test first, watch it fail, write the minimum code to make it pass, then refactor. This cycle has a name: red, green, refactor.

## The cycle

{{% steps %}}

### Red

Write a test for behaviour that doesn't exist yet. Run it. It fails. This confirms the test is actually checking something and that the behaviour isn't accidentally already there.

```python
def test_empty_cart_has_zero_total():
    cart = Cart()
    assert cart.total == 0
```

### Green

Write the simplest code that makes the test pass. Don't worry about elegance yet.

```python
class Cart:
    @property
    def total(self):
        return 0
```

### Red again

Write the next test. It fails because the implementation is still a stub.

```python
def test_cart_total_sums_item_prices():
    cart = Cart()
    cart.add(Item("Shirt", 50))
    cart.add(Item("Socks", 10))
    assert cart.total == 60
```

### Green again

Make it work for real.

```python
class Cart:
    def __init__(self):
        self.items = []

    def add(self, item):
        self.items.append(item)

    @property
    def total(self):
        return sum(item.price for item in self.items)
```

### Refactor

Both tests pass. Now clean up: remove duplication, improve naming, restructure. The tests give you confidence that your changes haven't broken anything.

{{% /steps %}}

Each cycle is small. You're never far from working code.

## TDD as a design tool

The part of TDD that gets talked about least is how it influences design. When you write the test first, you experience your code as a caller before you write it as an implementer. This tends to produce better APIs.

If the test is hard to write, that's immediate feedback that the interface is awkward, or that the function you're about to build has too many responsibilities. You find these problems before you've written the implementation, when they're cheapest to fix.

TDD also nudges you toward smaller functions with fewer dependencies, because those are easier to test. It naturally pushes code toward the patterns discussed in [Design for Testability](/testing/design-and-architecture/design-for-testability/) and [Functional Core, Imperative Shell](/testing/design-and-architecture/functional-core-imperative-shell/).

## When TDD works well

TDD is most useful when:

- The requirements are clear enough to express as a test before writing code
- You're building logic-heavy code where the inputs and outputs are well-defined
- You're working on a codebase with existing tests where the feedback loop is fast
- You want to avoid over-engineering, because TDD forces you to only write code that a test demands

## When TDD is harder to apply

TDD isn't equally effective everywhere:

- **Exploratory work.** When you're experimenting to figure out what you even want, writing tests first can feel like premature commitment. Sometimes you need to spike something out and write tests afterwards.

- **UI and visual code.** Testing that a button appears in the right place or that an animation looks correct is hard to express as a failing test. You often need to see it to evaluate it.

- **Integration-heavy code.** If the code is mostly wiring, calling APIs and shuttling data between systems, the interesting failures are in the integration, not the logic. Writing a unit test first for a function that's basically a pass-through doesn't add much.

- **Unfamiliar domains.** When you don't yet understand the problem well enough to specify the behaviour, tests-first can lead you in the wrong direction. It's sometimes better to prototype first and add tests once the shape of the solution is clearer.

## TDD doesn't mean 100% test-first

Most developers who practice TDD don't do it for every single line of code. They use it when it helps and skip it when it doesn't. A strict red-green-refactor cycle for a simple getter method is overhead for no benefit. TDD for a complex pricing calculation with edge cases and business rules is where it pays off.

The value of TDD is the discipline of thinking about behaviour before implementation, and the guarantee that your code is testable by construction. You can get most of that value without treating it as a rigid rule.
