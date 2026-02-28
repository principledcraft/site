---
title: Why We Test
type: docs
weight: 1
---

Software that works today will need to change tomorrow. Features get added, bugs surface, requirements shift. Without tests, every change is a gamble. With them, anyone on the team can modify code and know quickly whether they broke something.

## Fearless refactoring

A good test suite lets you change code without fear. When tests cover the important behaviour of your system, refactoring becomes routine. A junior developer, a new team member, or you six months from now can restructure internals and know within seconds whether anything broke.

Without tests, teams stop refactoring. Code stagnates, workarounds pile up, and the codebase gets harder to work with. Tests are what keep code malleable.

## What testing can and cannot tell you

[Edsger Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra) put it well:

> "Testing shows the presence, not the absence, of bugs."

A passing test suite means your code handles the scenarios you thought to check. It says nothing about the ones you missed.

That distinction matters more than it sounds. A passing suite doesn't prove your code is correct. It proves the scenarios you thought to check work the way you expected. Whether you checked the right scenarios is a separate question entirely.

## Tests as documentation

Well-written tests double as documentation. A new developer reading a well-named test learns what the function does, what inputs it handles, and what edge cases matter, without opening the implementation file.

This only works when tests are maintained alongside the code they cover. Neglected tests become noise, and noisy tests get ignored. But when you treat them as a first-class part of the codebase, they pay for themselves many times over.
