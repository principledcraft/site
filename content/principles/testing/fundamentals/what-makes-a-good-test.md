---
title: What Makes a Good Test
type: docs
weight: 3
---

Not all tests are equally useful. A test that runs slowly, breaks for no reason, or passes regardless of whether the code works is worse than no test at all. It wastes time and erodes trust in the suite.

Good tests share certain properties. The catch is that you can't maximise all of them at once.

## Fast

Tests that take minutes to run don't get run often. Developers skip them locally and only hear about failures from CI, by which point they've moved on to something else.

Fast tests become part of the rhythm. When the suite runs in a few seconds, people run it after every change without thinking about it.

## Isolated

Each test should be independent. It shouldn't matter what order they run in, or whether other tests pass or fail. A test that depends on state left behind by a previous test will eventually fail for reasons nobody can explain.

Each test sets up its own state and either cleans up after itself or, better yet, doesn't share mutable state in the first place. When tests share a database, use transactions or fresh data for each run.

## Deterministic

A test should produce the same result every time. Flaky tests, ones that pass sometimes and fail sometimes with no code change, will ruin a test suite. Teams start ignoring failures ("oh, that one's flaky, just re-run it"), and then real failures get ignored too.

Common sources of flakiness: relying on wall clock time, depending on network calls, assuming execution order for concurrent code, sharing mutable state between tests.

## Readable

A test should be easy to understand on its own. Someone reading it for the first time should be able to tell what's being tested and what the expected outcome is. If understanding a test requires tracing through shared fixtures and helper functions across multiple files, it's lost its value as documentation.

This sometimes means tolerating duplication. Three similar tests with clear inline setup are often better than three tests sharing a complex fixture nobody remembers writing.

## Meaningful

A test should verify something that matters. A test that asserts a function was called but doesn't check the result tells you nothing about correctness. A test that checks a return type but not its contents is barely better.

Here's a useful litmus test: could you break the implementation and still have this test pass? If yes, the test isn't earning its keep.

## The tensions

These properties pull against each other. A perfectly isolated test might need extensive setup, making it slower. Eliminating shared state can force duplication that hurts readability. Making a test more meaningful might require real dependencies, which introduces flakiness.

[Vladimir Khorikov](https://enterprisecraftsmanship.com/)'s [Unit Testing: Principles, Practices, and Patterns](/books/unit-testing-principles-practices-and-patterns/) frames this well. He identifies four pillars of a good test: protection against regressions, resistance to refactoring, fast feedback, and maintainability. You can't have all four at maximum, so the skill is knowing which to prioritise for a given test.

A unit test might sacrifice some regression protection (it can't catch integration bugs) in exchange for speed and maintainability. An integration test gives up fast feedback to get better protection. Neither is wrong. They're different tradeoffs for different situations.

Knowing what you're trading away matters more than chasing perfection on every axis.
