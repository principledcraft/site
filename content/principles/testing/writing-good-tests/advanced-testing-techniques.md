---
title: Advanced Testing Techniques
type: docs
weight: 4
---

Beyond unit, integration, and end-to-end tests, there are techniques that fill gaps conventional tests leave open.

## Property-based testing

Traditional tests check specific examples: given this input, expect this output. Instead of doing that, you define a property that should always hold, and the framework generates hundreds of random inputs to try to break it.

```python
from hypothesis import given, strategies as st

@given(st.lists(st.integers()))
def test_sorting_preserves_length(numbers):
    assert len(sorted(numbers)) == len(numbers)


@given(st.lists(st.integers()))
def test_sorting_produces_ordered_output(numbers):
    result = sorted(numbers)

    for i in range(len(result) - 1):
        assert result[i] <= result[i + 1]
```

Property-based tests are good at finding edge cases you wouldn't think to write by hand: empty lists, negative numbers, huge values. The downside is that failures can be harder to diagnose. It takes practice to figure out which properties are actually worth asserting.

## Fuzzing

Fuzzing takes the "throw random data at it" idea further. Where property-based testing generates structured inputs within defined types, fuzzers feed semi-random or mutated data directly to your code. They're mainly looking for crashes and security holes.

Fuzzing works best for code that processes untrusted input: parsers, deserializers, file format handlers. It's less useful for business logic where the input space is already well-defined. Setting up a useful fuzz test also takes more effort than a property-based test.

## Approval testing

Also called golden master tests or snapshot tests. Instead of writing assertions about specific values, you capture the output of your code and save it as a reference file. Future runs compare the current output against that reference and fail if anything changed.

```python
def test_invoice_rendering(approval):
    invoice = generate_invoice(
        customer="Acme Corp",
        items=[("Widget", 3, 9.99), ("Gadget", 1, 24.99)],
    )

    approval.verify(invoice.render())
```

These are particularly useful when working with legacy code that has no tests. You can wrap existing behaviour in approval tests without needing to understand every detail of what the code does. Capture the current output as a baseline, then refactor knowing you'll catch unintended changes. It's often the only realistic way to start adding tests to a codebase that was never written with them in mind.

The downside: they're brittle if the output format changes often, and they tell you nothing about whether the captured output is actually *correct*, only that it hasn't changed.
